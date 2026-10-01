<p align="center">
  <img src="images/icon.png" width="96" alt="Pipster icon">
</p>

<h1 align="center">Pipster</h1>

<p align="center">
  Readable item, fluid, gas and energy pipes for Minecraft, with filters right at the port<br>
  and channel-based tesseracts. For Fabric and NeoForge.
</p>

<p align="center">
  <a href="#downloads">Download</a> ·
  <a href="#version-status">Versions</a> ·
  <a href="../../wiki">Wiki</a> ·
  <a href="api/README.md">API for mod developers</a> ·
  <a href="#license">License</a>
</p>

> **Version 1.0.0**, the first release. For Minecraft 1.21.1 to 26.3 on Fabric and NeoForge; see
> [Version status](#version-status).

This repository holds the documentation, the [wiki](../../wiki), the
[issue tracker](../../issues) and the [API for mod developers](api/README.md). The mod itself
is downloaded from Modrinth or CurseForge. Pipster is not open source; see [License](#license).

![Pipster pipes between two chests, cauldrons and tesseracts](images/01-machines.jpg)

## What Pipster is

Pipster is a transport mod that tries to stay easy to read. Every resource type has its own
pipe, every pipe looks like a pipe (no full blocks), and everything you configure lives
directly at the port where the pipe meets a machine.

* **Four transport kinds:** item pipes, fluid pipes, gas pipes and energy cables. Different
  kinds never connect to each other.
* **Five tiers:** Copper, Reinforced, Precision, Resonant and Quantum. Upgrade pipes in place
  with an upgrade bolt; the pipe keeps its settings and contents.
* **Configure ports with the Conduit Wrench:** mode (insert, extract, both, off), redstone
  control, priority, distribution (round robin, nearest first, sticky) and copy/paste profiles.
* **Filter cards instead of filter blocks:** put a card into the port itself. Basic (9 rules),
  Advanced (27 rules: tags, mod namespace, components) and Precision (54 rules: also "keep N in
  the source" and "fill the target up to N"). The Rule Card adds port rate limits and an energy
  reserve.
* **Tesseracts:** send resources through named channels. The Resonant Tesseract works over any
  distance in the same dimension. The Quantum Tesseract works across all dimensions of a
  server. Channels are private, invite-only or public.
* **Honest throughput:** every pipe segment has one shared per-tick budget. A single Copper
  segment limits a whole path, even between Quantum ports. There is no creative tier that
  ignores limits.

![All 64 connection shapes of item pipes](images/02-item-pipe-shapes.jpg)

## Tiers

Rates are per pipe segment and tick, shared by all transfers through that segment. They are
balance values, not measured results.

| Tier | Items/t | Fluid mB/t | Gas units/t | Energy E/t | Item batch |
|---|---:|---:|---:|---:|---:|
| T1 Copper | 2 | 100 | 100 | 1,000 | 8 |
| T2 Reinforced | 8 | 400 | 400 | 8,000 | 32 |
| T3 Precision | 32 | 1,600 | 1,600 | 64,000 | 64 |
| T4 Resonant | 128 | 6,400 | 6,400 | 512,000 | 256 |
| T5 Quantum | 512 | 25,600 | 25,600 | 4,096,000 | 1,024 |

## Works with other mods

Pipster talks to other mods through the standard interfaces of each loader. It has no hard
dependency on any content mod.

| Resource | Fabric | NeoForge |
|---|---|---|
| Items | Fabric Transfer API | NeoForge item capability |
| Fluids | Fabric Transfer API | NeoForge fluid capability |
| Energy | Team Reborn Energy (included) | NeoForge energy capability |
| Gas | Pipster gas API, plus fluids tagged `#pipster:gaseous` | Pipster gas API, plus fluids tagged `#pipster:gaseous` |

Mod developers find the exact interfaces per version in the [API documentation](api/README.md).

What this means in practice:

* Any block that exposes these interfaces can be connected. That is not a promise that every
  mod works. Besides the automated tests, the release was played with Energized Power, Storage
  Drawers, Oritech, Create, Immersive Engineering and Sophisticated Storage.
* **Mekanism chemicals are not supported yet** (no adapter). Mekanism gases do not travel
  through Pipster gas pipes; Mekanism items, fluids and energy work through the normal interfaces.
* On Fabric, energy follows the push convention: generators push into cables, and a port must be
  set to Extract or Both to accept that push.

### Built-in optional integrations

These mods are **never required**. Install them and Pipster uses them; without them nothing
changes. Each integration is included for the Minecraft versions where the other mod has a
release (see the [wiki](../../wiki/Supported-Versions#integrations)).

| Mod | What you get |
|---|---|
| [Jade](https://modrinth.com/mod/jade) | Look at a pipe or tesseract: port mode and status, bound channels (names only if you have access). |
| [CC: Tweaked](https://modrinth.com/mod/cc-tweaked) | Computers read pipes and tesseracts (ports, status, buffers). With the redstone mode "Computer" they can switch ports. |
| [Open Parties and Claims](https://modrinth.com/mod/open-parties-and-claims), [FTB Chunks](https://www.curseforge.com/minecraft/mc-mods/ftb-chunks-forge) | Claim protection: pipes cannot pull from or push into blocks in someone else's claim. |
| FTB Teams, Open Parties and Claims parties | Team sharing: teammates share channels, tesseracts and pipes, also for things placed before they joined. |
| LuckPerms and other permission mods | Permission nodes `pipster.admin` and `pipster.command`. Without a permission mod, operators (level 2) keep these rights. |

The Conduit Wrench is also tagged `c:tools/wrench`, so other mods recognise it as a wrench.

## Version status

All versions below are tested. Some jars cover several Minecraft versions; such a jar was started
and tested unchanged on **every** version it lists.

| Minecraft | Fabric jar | NeoForge jar |
|---|---|---|
| 1.21.1 | `pipster-1.0.0+mc1.21.1-fabric.jar` | `pipster-1.0.0+mc1.21.1-neoforge.jar` |
| 1.21.2 – 1.21.4 | `pipster-1.0.0+mc1.21.2-1.21.4-fabric.jar` | `pipster-1.0.0+mc1.21.2-1.21.4-neoforge.jar` ¹ |
| 1.21.5 | `pipster-1.0.0+mc1.21.5-fabric.jar` | `pipster-1.0.0+mc1.21.5-neoforge.jar` |
| 1.21.6 – 1.21.8 | `pipster-1.0.0+mc1.21.6-1.21.8-fabric.jar` | `pipster-1.0.0+mc1.21.6-1.21.8-neoforge.jar` ¹ |
| 1.21.9 | `pipster-1.0.0+mc1.21.9-1.21.10-fabric.jar` | `pipster-1.0.0+mc1.21.9-neoforge.jar` ¹ |
| 1.21.10 | `pipster-1.0.0+mc1.21.9-1.21.10-fabric.jar` | `pipster-1.0.0+mc1.21.10-neoforge.jar` |
| 1.21.11 | `pipster-1.0.0+mc1.21.11-fabric.jar` | `pipster-1.0.0+mc1.21.11-neoforge.jar` |
| 26.1, 26.1.1 | `pipster-1.0.0+mc26.1-26.1.2-fabric.jar` | `pipster-1.0.0+mc26.1-26.1.1-neoforge.jar` ¹ |
| 26.1.2 | `pipster-1.0.0+mc26.1-26.1.2-fabric.jar` | `pipster-1.0.0+mc26.1.2-neoforge.jar` |
| 26.2 | `pipster-1.0.0+mc26.2-fabric.jar` | `pipster-1.0.0+mc26.2-neoforge.jar` |
| 26.3 | `pipster-1.0.0+mc26.3-fabric.jar` | `pipster-1.0.0+mc26.3-neoforge.jar` ¹ |

¹ For some of these Minecraft versions NeoForge only had beta builds on the test date (1.21.2,
1.21.6, 1.21.7, 1.21.9, 26.1, 26.1.1, 26.3). The jar was tested with that beta.

"Tested" means, for every listed version: client and dedicated server started, and all automated
game tests passed (item, fluid, energy and gas transport, throughput limits, filters, tesseracts,
claims and teams, break/place with buffered contents, save/load, network packet checks), on both
loaders. The integrations were started with the real mods on every version where those exist.
On top of that, every row was played by hand before the release. The exact loader versions are
listed in the wiki under [Supported Versions](../../wiki/Supported-Versions).

## Performance

Pipster does no work per pipe per tick. It keeps one transport graph per dimension and
resource type on the server, rebuilds it in bounded steps, and never loads chunks. On the
benchmark machine (Intel Core i7-13700H, dedicated server, Minecraft 1.21.1 and 26.3, both
loaders):

| Scene | Pipster time per tick (p95) |
|---|---|
| 10,000 idle pipes | about 0.01 ms |
| 10,000 pipes, 100 active ports | 0.8 – 1.4 ms |
| 50,000 pipes, 500 active ports | 3.7 – 5.2 ms |
| network that splits and merges every second | 0.03 – 0.05 ms |

These numbers hold for that machine and those scenes only. They are no guarantee of 20 TPS on
other hardware or in other worlds. The method is described in the wiki under
[Performance](../../wiki/Performance).

## Server and multiplayer

* Everything is decided on the server. Clients only see pipe shapes and the GUI data they are
  allowed to see.
* Every GUI action is checked on the server: session, distance, build permission, tesseract
  channel access, and a revision number so stale screens cannot overwrite newer settings.
* Tesseract channels only work while all involved chunks are loaded. Pipster never loads chunks
  on its own.
* Creative pick-block never copies buffered resources or filter cards.
* With a claim mod installed, pipes respect claims; with a team mod, teammates share ownership;
  with a permission mod, the admin rights can be given per player (see above).
* Server settings live in `config/pipster-server.properties` (see the
  [wiki](../../wiki/Configuration)).

## Downloads

[Modrinth](https://modrinth.com/project/pipster)
[CurseForge](https://www.curseforge.com/minecraft/mc-mods/pipster/)

Please download Pipster only from these pages. Pick the file for your exact Minecraft version
and loader.

## Documentation

* [Wiki](../../wiki): how to play, recipes, tesseracts, configuration, FAQ
* [API for mod developers](api/README.md): gas handlers, endpoint providers, CC: Tweaked methods

## Reporting bugs

Please open an [issue](../../issues/new/choose) with:

* Minecraft version, loader (Fabric/NeoForge) and loader version
* the Pipster jar name (it contains the Minecraft version or range)
* the other mods involved, if the problem is with a specific machine
* `logs/latest.log`, and a crash report if there is one

## License

Pipster is **not open source**. All rights reserved by cptgummiball; see [`LICENSE`](LICENSE).
In short:

* You may play with Pipster, use it on any server and show it in videos and streams.
* You may put Pipster into **modpacks**, as long as the jar files stay exactly as published.
* You may **not** modify, decompile, re-upload or otherwise redistribute Pipster, or use its
  textures and models elsewhere.
* Mod developers may compile against the [Pipster API](api/README.md) to make their mods work
  with Pipster.
* For anything else, open a [permission request](../../issues/new?template=permission_request.yml).

Pipster is made by **cptgummiball**. It is inspired by the readability of classic pipe mods, but
contains no code or assets from other mods. Pipster is not an official Minecraft product and is
not associated with Mojang or Microsoft.
