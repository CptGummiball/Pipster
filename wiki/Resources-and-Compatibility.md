# Resources and Compatibility

Pipster does not need any other content mod. It connects to other mods only through the
standard transfer interfaces of each loader. Any block that offers these interfaces on a side
can be connected to a matching pipe.

## Interfaces Pipster uses

| Resource | Fabric | NeoForge 1.21.1 – 1.21.8 | NeoForge 1.21.9 – 1.21.11 and 26.x |
|---|---|---|---|
| Items | Fabric Transfer API `ItemStorage` | `IItemHandler` capability | `ResourceHandler<ItemResource>` |
| Fluids | Fabric Transfer API `FluidStorage` | `IFluidHandler` capability | `ResourceHandler<FluidResource>` |
| Energy | Team Reborn Energy `EnergyStorage` | `IEnergyStorage` capability | `EnergyHandler` |
| Gas | Pipster gas API | Pipster gas API | Pipster gas API |

Units:

* Items: count. Item components (enchantments, names, custom data) are always kept.
* Fluids: stored exactly (81,000 units per bucket, the Fabric droplet). NeoForge fluid handlers
  work in millibuckets; Pipster never loses fractions, it keeps them until they add up.
* Energy: 1 E = 1 FE = 1 Team Reborn Energy unit. That is a balance decision, not a physical
  conversion.

## Gas

Minecraft has no built-in gas, and there is no common gas interface on both loaders. Pipster
therefore supports gas in two ways:

1. **The Pipster gas API** (`pipster:gas`): mods can register gases and offer a gas handler.
   See [For Mod Developers](For-Mod-Developers).
2. **Gaseous fluids** (`pipster:fluid_gas`): any fluid listed in the fluid tag
   `#pipster:gaseous` travels in **gas pipes** instead of fluid pipes. The tag is empty by
   default; add fluids with a datapack.

A gas pipe network carries only one of these kinds at a time. Pipster never converts between
them.

### Mekanism

**Mekanism chemicals are not supported yet.** There is no adapter for Mekanism's chemical
system, so Mekanism gases do not travel through Pipster gas pipes. Mekanism items, fluids and
energy use the normal interfaces above.

## What can go wrong with other mods

* **Fabric energy is push-based.** Generators push energy into cables. Pipster never pulls
  energy out of a Fabric energy storage. Set the generator's port to Extract or Both so the push
  is accepted.
* **Quarantine.** If a block reports impossible numbers (for example, it claims to accept more
  than it was offered or hands out a different item than requested), Pipster pauses that
  connection for a while (default one minute) and shows "Quarantined: faulty foreign handler".
  Pipster still keeps every resource it already holds.
* **Quantity rules** need blocks that report their contents reliably. Otherwise the rule
  blocks, see [Filter and Rule Cards](Filter-and-Rule-Cards).
* **Crashes and exactly-once.** A transfer is either completed or rolled back within a tick.
  But Minecraft saves chunks separately, so if the server process is killed mid-tick (power loss,
  task manager), items that were moving between two machines at that moment can be lost or
  duplicated. That is a Minecraft limitation and applies to any transport mod. Normal saving,
  stopping and chunk unloading are handled correctly.

## Tested

Automated tests use vanilla blocks (chests, cauldrons) and Pipster's own test blocks for energy
and gas. **No other mods are installed in these tests.** If you find a mod that does not work
with Pipster, please report it on the issue tracker.
