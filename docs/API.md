# Pipster Integration API (API version 1)

Author: cptgummiball · Package `dev.cptgummiball.pipster.api` · Mod id `pipster`

Pipster does **not** require other mods to link against it. Blocks that implement the native
loader interfaces are used directly. The Pipster API exists only for things without a native
interface (the `pipster:gas` domain) and for optional custom endpoint providers.

## 1. What Pipster talks to (per version family)

| Resource | Fabric 1.21.1 | NeoForge 1.21.1 |
|---|---|---|
| Items | `ItemStorage.SIDED` (`Storage<ItemVariant>`), transactional | `Capabilities.ItemHandler.BLOCK` (`IItemHandler`), simulate/execute |
| Fluids | `FluidStorage.SIDED` (`Storage<FluidVariant>`), droplets | `Capabilities.FluidHandler.BLOCK` (`IFluidHandler`), mB |
| Energy | Team Reborn Energy 4.1.0 `EnergyStorage.SIDED` | `Capabilities.EnergyStorage.BLOCK` (`IEnergyStorage`, FE) |
| Gas `pipster:gas` | `BlockApiLookup<GasHandler, Direction>` id `pipster:gas_handler` | `BlockCapability<GasHandler, Direction>` id `pipster:gas_handler` |
| Gas `pipster:fluid_gas` | fluid storage, only fluids in `#pipster:gaseous` | fluid handler, only fluids in `#pipster:gaseous` |
| Gas `mekanism:chemical` | not available (no verified Fabric distribution) | adapter **not built yet** (see IMPLEMENTATION_STATUS.md) |

Other families will list their verified interfaces here once they are built and tested
(e.g. NeoForge 21.9+ `ResourceHandler` transfer API, Team Reborn Energy 5.x on 26.x).

### Semantics Pipster relies on

* **Transactional APIs (Fabric)**: Pipster plans a transfer in a nested transaction and commits
  exactly once. Pipster's own budgets and buffers participate in the same transaction.
* **Simulate/execute APIs (NeoForge 1.21.1, `GasHandler`)**: simulations are advisory. Pipster
  extracts into a buffer it owns, then inserts; residue is returned to the source or stays in the
  buffer. Handlers that return more than requested, other variants than requested, or that report
  more accepted than offered are **quarantined** (default 1200 ticks, config `quarantineTicks`).
* **Energy push convention (Fabric)**: generators push into cables; Pipster never pulls energy
  from Fabric energy storages. A port must be in `EXTRACT` or `BOTH` mode to accept pushes.
  On NeoForge, EXTRACT ports may actively pull if `canExtract()` is true.
* **Units**: items = count; fluids = droplets internally (81,000 per bucket, 81 per mB; NeoForge
  mB endpoints only ever see whole mB, sub-mB remainders stay with their owner); energy E = 1 FE =
  1 TR unit (a balance decision, not a physical conversion); `pipster:gas` = the unit declared
  by the registrant, never converted.
* **Components** are part of every item/fluid variant and are preserved.

## 2. The `pipster:gas` domain

```java
// during mod initialisation (both loaders)
GasRegistry.register(new GasType("examplemod:steam", "mB"));
```

Expose a `GasHandler` on your block entity:

```java
// Fabric
BlockApiLookup<GasHandler, Direction> GAS =
    BlockApiLookup.get(ResourceLocation.fromNamespaceAndPath("pipster", "gas_handler"), GasHandler.class, Direction.class);
GAS.registerForBlockEntity((be, side) -> be.gasHandler(side), MY_BE_TYPE);

// NeoForge (RegisterCapabilitiesEvent)
BlockCapability<GasHandler, Direction> GAS =
    BlockCapability.createSided(ResourceLocation.fromNamespaceAndPath("pipster", "gas_handler"), GasHandler.class);
event.registerBlockEntity(GAS, MY_BE_TYPE, (be, side) -> be.gasHandler(side));
```

`GasHandler` contract (simulate/execute):

* `insert(gasId, components, amount, simulate)` returns the accepted amount `0..amount`.
* `extract(gasId, components, amount, simulate)` removes **only exactly this gas** and returns
  `0..amount`.
* `simulate == true` must not change state.
* `components` is a canonical string owned by the registrant (`""` for none). It is part of the
  identity.

A gas pipe network carries exactly one domain at a time: `pipster:gas`, `pipster:fluid_gas` or
(later) `mekanism:chemical`. The first successful transfer chooses it. There is no automatic
conversion between domains, even for identical ids.

## 3. Datapack hooks

* `#pipster:gaseous` (fluid tag): fluids listed here travel in **gas** pipes (domain
  `pipster:fluid_gas`) and no longer in fluid pipes. Default: empty.
* All recipes are normal datapack recipes. Upgrade recipes use `pipster:empty_contents_shaped`
  (a shaped recipe that rejects ingredients with stored contents or block-entity data).

## 4. `TransportEndpointProvider`

`TransportEndpointProvider<C, H>` is the versioned contract for custom providers (`apiVersion()`,
`kind()`, `domain()`, `semantics()`, `find(context, invalidationCallback)`). In API version 1,
the only supported handler type is `GasHandler` for the `pipster:gas` domain. Items, fluids and
energy use the native loader interfaces listed above.

Registration (both loaders, 1.21.1 and 26.3), during mod initialisation:

```java
PipsterEndpointProviders.register(new TransportEndpointProvider<PipsterEndpointProviders.Context, GasHandler>() {
    public int apiVersion() { return PipsterApi.API_VERSION; }
    public ResourceKind kind() { return ResourceKind.GAS; }
    public ResourceDomain domain() { return ResourceDomain.GAS; }
    public TransferSemantics semantics() { return TransferSemantics.SIMULATE_EXECUTE; }
    public GasHandler find(PipsterEndpointProviders.Context ctx, Runnable invalidate) {
        return /* handler at ctx.level()/ctx.pos() for side ctx.side(), or null */;
    }
});
```

* `PipsterEndpointProviders` lives in `dev.cptgummiball.pipster.integration` of the loader
  artifact (it depends on Minecraft types, so it is not part of the Minecraft-free API module).
* `register` rejects providers with a different `apiVersion()` or any kind/domain other than
  `GAS`/`pipster:gas` (`IllegalArgumentException`).
* Providers are asked before the native lookup. Pipster only asks for loaded positions, on the
  server thread. `find` must be cheap, must not load chunks, and must call `invalidate` when a
  returned handler becomes stale (Pipster also re-queries on neighbour and block updates).
* Returned handlers are subject to the same checks as all `SIMULATE_EXECUTE` handlers: over-reports
  and wrong-resource extractions put the endpoint into quarantine.
* Covered by the GameTest `gasIntoProviderEndpoint` (test content only).

## 5. Stability

* `API_VERSION` changes on every binary-incompatible change of `dev.cptgummiball.pipster.api`.
* Everything outside `dev.cptgummiball.pipster.api` is internal and may change without notice.
* Saved data formats are documented in `docs/SAVE_FORMAT.md`.
