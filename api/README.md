# Pipster API for mod developers

API version **1** · package `dev.cptgummiball.pipster.api` · mod id `pipster`

**Most mods need nothing from this page.** Pipster pipes connect to any block that offers the
standard interfaces of the loader (see [below](#1-interfaces-pipster-uses)). You only need the
Pipster API for:

* **gas**: neither Fabric nor NeoForge has a gas interface. Pipster brings its own small one
  (`pipster:gas`).
* **endpoint providers**: if you want to offer gas handlers for blocks that are not yours.

## Getting the API

| What | Where | Use |
|---|---|---|
| `pipster-api-1.jar` + `pipster-api-1-javadoc.jar` | [Releases](../../../releases) of this repository | compile-only dependency for gas handlers and gas registration |
| the Pipster mod jar | Modrinth Maven (`maven.modrinth:pipster:<version>`) | compile-only, only needed for `PipsterEndpointProviders` |

Example (Gradle). Put the API jar into a `libs/` folder of your project:

```groovy
dependencies {
    compileOnly files("libs/pipster-api-1.jar")
    // only for endpoint providers:
    // modCompileOnly "maven.modrinth:pipster:<version>"   // Fabric (Loom)
    // compileOnly    "maven.modrinth:pipster:<version>"   // NeoForge (ModDevGradle)
}
repositories {
    maven { url = "https://api.modrinth.com/maven" }
}
```

**Do not ship the API or any Pipster files in your mod.** Players install Pipster separately.
If Pipster is optional for your mod, only touch Pipster classes after checking that the mod
`pipster` is loaded (`FabricLoader.getInstance().isModLoaded("pipster")` /
`ModList.get().isLoaded("pipster")`).

The Pipster license (see [`LICENSE`](../LICENSE), section 5) allows you to compile against the
API and to publish mods that use it. It does not allow modifying or redistributing the API.

## 1. Interfaces Pipster uses

| Resource | Fabric (all versions) | NeoForge 1.21.1 – 1.21.8 | NeoForge 1.21.9 – 1.21.11 and 26.x |
|---|---|---|---|
| Items | `ItemStorage.SIDED` (`Storage<ItemVariant>`) | `Capabilities.ItemHandler.BLOCK` (`IItemHandler`) | `Capabilities.Item.BLOCK` (`ResourceHandler<ItemResource>`) |
| Fluids | `FluidStorage.SIDED` (`Storage<FluidVariant>`) | `Capabilities.FluidHandler.BLOCK` (`IFluidHandler`) | `Capabilities.Fluid.BLOCK` (`ResourceHandler<FluidResource>`) |
| Energy | Team Reborn Energy `EnergyStorage.SIDED` | `Capabilities.EnergyStorage.BLOCK` (`IEnergyStorage`) | `Capabilities.Energy.BLOCK` (`EnergyHandler`) |
| Gas | Pipster `GasHandler` via `BlockApiLookup` | Pipster `GasHandler` via `BlockCapability` | Pipster `GasHandler` via `BlockCapability` |

Team Reborn Energy versions Pipster was tested with: 4.1.0 (1.21.1–1.21.4), 4.2.0
(1.21.5–1.21.11), 5.0.0 (26.x).

### Semantics Pipster relies on

* **Transactional APIs** (Fabric Transfer API, NeoForge `ResourceHandler`/`EnergyHandler`):
  Pipster plans a transfer in a nested transaction and commits exactly once.
* **Simulate/execute APIs** (NeoForge `IItemHandler` etc. up to 1.21.8, Pipster `GasHandler`):
  simulations are only a hint. Pipster extracts into a buffer it owns, then inserts. Anything
  left over goes back to the source or stays in the buffer. It is never deleted.
* **Quarantine:** handlers that return more than requested, hand out something other than
  requested, or report more accepted than offered are paused for a while (default 1200 ticks).
* **Fabric energy push convention:** generators push into cables. Pipster never pulls energy out of
  a Fabric energy storage. A port must be set to Extract or Both to accept the push. On NeoForge,
  Extract ports may pull if the storage allows extraction.
* **Units:** items = count; fluids internally in droplets (81,000 per bucket). NeoForge mB handlers
  only ever see whole mB; fractions stay with their owner. Energy: 1 E = 1 FE = 1 Team Reborn
  Energy unit. Gas: the unit your gas declares, never converted.
* **Components** (enchantments, names, custom data) are part of every item and fluid variant and
  are always kept.
* Pipster only queries **loaded** positions, only on the server thread, and never loads chunks.

## 2. Gas (`pipster:gas`)

### Register a gas

```java
// during mod initialisation, on both loaders
GasRegistry.register(new GasType("examplemod:steam", "mB"));
```

The unit string is yours. Gas pipe throughput counts in this unit.

### Offer a gas handler

```java
// Fabric
BlockApiLookup<GasHandler, Direction> GAS = BlockApiLookup.get(
        ResourceLocation.fromNamespaceAndPath("pipster", "gas_handler"), GasHandler.class, Direction.class);
GAS.registerForBlockEntity((be, side) -> be.gasHandler(side), MY_BE_TYPE);

// NeoForge, in RegisterCapabilitiesEvent
BlockCapability<GasHandler, Direction> GAS = BlockCapability.createSided(
        ResourceLocation.fromNamespaceAndPath("pipster", "gas_handler"), GasHandler.class);
event.registerBlockEntity(GAS, MY_BE_TYPE, (be, side) -> be.gasHandler(side));
```

On Minecraft 1.21.11 and 26.x the class is called `Identifier` instead of `ResourceLocation`.

### The `GasHandler` contract

```java
public interface GasHandler {
    int tanks();
    GasStack gas(int tank);
    long capacity(int tank);
    long insert(String gasId, String components, long amount, boolean simulate);   // returns 0..amount
    long extract(String gasId, String components, long amount, boolean simulate);  // returns 0..amount
}
```

* `insert` returns how much was accepted, between 0 and `amount`.
* `extract` removes **only exactly this gas** and returns how much, between 0 and `amount`.
* With `simulate == true` nothing may change.
* `components` is your own canonical string for gas variants (`""` if there are none). It is part
  of the gas identity.

### Domains

A gas pipe network carries exactly one gas domain at a time. The first successful transfer picks it:

* `pipster:gas`: gases from this API
* `pipster:fluid_gas`: fluids in the fluid tag `#pipster:gaseous` (a datapack can add fluids;
  default empty). These go through gas pipes instead of fluid pipes.

Pipster never converts between domains, even for identical ids. Mekanism chemicals are not
supported yet.

## 3. Endpoint providers

Endpoint providers let you offer gas handlers for blocks you cannot change. They need the mod jar
at compile time (see [Getting the API](#getting-the-api)).

```java
PipsterEndpointProviders.register(new TransportEndpointProvider<PipsterEndpointProviders.Context, GasHandler>() {
    public int apiVersion() { return PipsterApi.API_VERSION; }
    public ResourceKind kind() { return ResourceKind.GAS; }
    public ResourceDomain domain() { return ResourceDomain.GAS; }
    public TransferSemantics semantics() { return TransferSemantics.SIMULATE_EXECUTE; }
    public GasHandler find(PipsterEndpointProviders.Context ctx, Runnable invalidate) {
        return /* handler at ctx.level() / ctx.pos() for side ctx.side(), or null */;
    }
});
```

* `PipsterEndpointProviders` is in package `dev.cptgummiball.pipster.integration` of the mod jar.
* `register` rejects providers with a different `apiVersion()` or any kind or domain other than
  `GAS` / `pipster:gas` (`IllegalArgumentException`).
* Providers are asked before the loader's own lookup. `find` must be cheap, must not load chunks,
  and must call `invalidate` when a handler it returned is no longer valid.
* Returned handlers go through the same checks (and quarantine) as all simulate/execute handlers.

## 4. Datapack hooks

* Fluid tag `#pipster:gaseous`: see [Domains](#domains).
* All recipes are normal datapack recipes. Upgrade recipes use the recipe type
  `pipster:empty_contents_shaped`: a shaped recipe that refuses ingredients which still carry
  buffered contents or block data.

## 5. Built-in integrations (no code needed)

These work without any code in your mod. They are only active when the other mod is installed.

### CC: Tweaked

Conduits and tesseracts are a generic peripheral with the additional type `pipster`
(`peripheral.find("pipster")`). Reading is always possible. Switching needs the port's (or
tesseract side's) redstone mode **Computer**, which a player with build rights sets in the port
screen. Channel names are never exposed, and channels cannot be selected from Lua. All methods run
on the server thread and stay within the one block.

| Method (target) | Returns / does |
|---|---|
| `getConduit()` (conduit) | `{kind, tier, rebuilding, network?, nodes?, ports?}` |
| `getPorts()` (conduit) | ports by side name: `{mode, status, priority, redstone, enabled?, quarantined, buffer = [{name, amount, unit}]}` |
| `getPort(side)` (conduit) | one port; error if there is none |
| `setPortEnabled(side, enabled)` (conduit) | switches the port on or off (redstone mode "computer" only) |
| `setPortMode(side, mode)` (conduit) | `insert`, `extract`, `both`, `disabled` (redstone mode "computer" only) |
| `getTesseract()` (tesseract) | `{quantum, item/fluid/gas/energy = {bound, sides = {side = {mode, status, ...}}}}` |
| `setSideEnabled(kind, side, enabled)` (tesseract) | switches a side on or off (redstone mode "computer" only) |

Sides are `down`, `up`, `north`, `south`, `west`, `east`. Amounts use display units (items, mB,
E). Status values are the port statuses in lower case, for example `active`, `source_empty`,
`targets_full`, `claim_protected`, `computer_off`.

### Permission nodes

`pipster.admin` (see and edit foreign private tesseract channels) and `pipster.command`
(`/pipster`). Fabric: through fabric-permissions-api. NeoForge: through NeoForge's permission
system. Without a permission tool, or with a node unset, the operator check (level 2) applies.

### Claims and teams

Pipster honours Open Parties and Claims and FTB Chunks claims for ports and pipe connections, and
treats members of one FTB Teams team or OPAC party as co-owners of channels, tesseracts and
conduits. Your mod does not need to do anything for this.

### Tags

* `c:tools/wrench` contains `pipster:conduit_wrench`. Pipster's own actions still require the
  Conduit Wrench.

## 6. Stability

* `PipsterApi.API_VERSION` changes with every incompatible change to the API package. The API jar
  carries the API version in its name.
* The only supported entry points are the package `dev.cptgummiball.pipster.api` and
  `dev.cptgummiball.pipster.integration.PipsterEndpointProviders`. Everything else is internal
  and can change in any release.

Questions and requests: please open an [issue](../../../issues/new/choose).
