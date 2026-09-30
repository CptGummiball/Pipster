# Pipster save format and migration notes

All Pipster data is versioned (`schema_version`). Loaders never auto-migrate worlds between
Minecraft **downgrades**. Back up worlds before updating Pipster or Minecraft.

## Block entity `pipster:conduit` (schema 1)

```
schema_version: int = 1
disabled: int                      # wrench-disabled arm bitmask (bit = Direction 3D data value), optional
sides: [                           # only sides that ever had a port, content or cards
  { side: int 0..5,
    cfg: PortConfig,               # see below
    buf: [BufferEntry],            # optional: resources owned by this port
    cards: {Items:[...]} }         # optional: physical filter card (slot 0), rule card (slot 1)
]
```

The block (kind + tier) is the block id, e.g. `pipster:fluid_pipe_t3`. The connection bits are the
block state (`north|south|east|west|up|down`, `waterlogged`). The transport graph is **not**
saved: it is rebuilt from loaded block entities.

### PortConfig (schema 1)

```
v: 1, mode: INSERT|EXTRACT|BOTH|DISABLED, rs: IGNORE|REQUIRE_SIGNAL|FORBID_SIGNAL, prio: int -99..99,
dist: ROUND_ROBIN|NEAREST|STICKY, rate: long (optional), reserve: long (optional), allow: bool,
rules: [{ i: 0..53, id?, tag?, ns?, c? (canonical components), qm?: SOURCE_RESERVE|TARGET_STOCK, q? }]
```

Unknown enum names fall back to defaults. Strings are length-checked (<= 256, components <= 4096).

### BufferEntry

```
d: domain id (minecraft:item | minecraft:fluid | pipster:energy | pipster:gas | pipster:fluid_gas | mekanism:chemical)
id: registry id string          # never numeric registry ids
c: DataComponentPatch (NBT)      # items/fluids, optional
sc: string                       # pipster:gas component string, optional
a: long amount                   # items: count, fluids: droplets (1/81,000 bucket), energy: E, gas: declared unit
f: int flags                     # 1 = arrived through a tesseract channel (may not enter another), 2 = pushed in
```

If an entry cannot be resolved (missing mod/fluid/gas, undecodable components, domain
reclassified by `#pipster:gaseous`), it is kept **verbatim** as a locked entry: it is never
delivered, never dropped, and written back unchanged. Re-adding the mod restores it.

## Block entity `pipster:tesseract` (schema 1)

```
schema_version: 1
placer: UUID (optional)
bindings: [{ kind: item|fluid|gas|energy, channel: UUID, by: UUID }]   # "by" = player whose access authorised it
sides: [{ kind, side: 0..5, cfg: PortConfig, buf?: [BufferEntry], cards?: {Items:[...]} }]
```

Bindings are re-validated against the channel ACL every service tick. A revoked binding is
offline, but it is not deleted.

## Saved data `pipster_channels` (overworld, schema 1)

```
schema_version: 1
channels: [{ id: UUID, kind, owner: UUID, name: string <= 32, visibility: PRIVATE|INVITED|PUBLIC, invited: [UUID] }]
```

There are no buffers here. Runtime membership comes only from loaded tesseracts.

## Items

* `pipster:stored_contents` (component): `{schema_version: 1, sides: [{side, buf:[BufferEntry]}]}`
  for conduits, and `{..., sides:[{kind, side, buf}]}` for tesseracts. It is created only by
  breaking a block with buffered content. It is applied once on placement. How it gets into the
  dropped item depends on the version, but the item format is the same everywhere:
  * 1.21.1: loot `minecraft:copy_components` from the block entity (implicit component).
  * 1.21.2 and later (bands `mc1212`, `mc26`): loot function `pipster:copy_stored_contents`.
    From 1.21.4 on, creative pick-block runs on the server and copies the block entity's
    implicit components. So buffers are **no** implicit component there, and pick-block data
    loses `buf` and `cards` in `removeComponentsFromTag`.
* `pipster:wrench_state` (component): `{active: 0..2, profiles: {p0|p1|p2: {kind, cfg: PortConfig}}}`
  holds settings only, never cards or resources.
* Block-entity data copied into items (creative pick-block with data) is sanitised on
  placement: buffers, cards, bindings and placer are removed.

## Version differences in the stored format

* 1.21.1 to 1.21.5 write block entities as NBT. 1.21.6 and later use `ValueOutput`/`ValueInput`,
  and 26.x does too. The field names and structure above are identical; only the API differs.
* A world may move to a **newer** Minecraft version with the matching Pipster artifact. Minecraft
  does not support moving to an older version.
  
## Known persistence limits

* Minecraft does not save foreign inventories and Pipster block entities atomically. An abrupt
  process kill between two chunk saves can lose or duplicate items that were in flight in that
  tick. Normal ticks, controlled save/stop and chunk unload/reload are covered by tests.
* Moving a world between loaders (Fabric ↔ NeoForge) uses the same ids and formats, but this has
  **not been tested yet**.
