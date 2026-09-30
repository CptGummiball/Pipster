# Supported Versions

Pipster ships **one jar per Minecraft version and loader**. Every jar declares exactly the one
Minecraft version it was tested on. There is no jar that claims a whole version range, because
Minecraft's internal code changes in almost every release.

## What "tested" means

For every version marked tested, the jar for exactly that version was:

1. built,
2. started as a client and as a dedicated server, on Fabric and on NeoForge,
3. run through all automated game tests on both loaders. They cover item, fluid, energy and gas
   transport, throughput limits, filters, tesseracts, breaking and placing pipes with buffered
   content, save/load, unloading and reloading, recipe and loot loading, the integration API
   and network packet checks.

Most versions were also checked visually in a real client (all 64 pipe shapes, machines, GUI).

"Tested" does **not** mean that every other mod works with Pipster. See
[Resources and Compatibility](Resources-and-Compatibility).

## Status

| Minecraft | Status | Notes |
|---|---|---|
| 1.21.1 | tested, benchmarked | |
| 1.21.2 | tested | NeoForge only as beta |
| 1.21.3 | tested | |
| 1.21.4 | tested | |
| 1.21.5 | tested | |
| 1.21.6 | tested | NeoForge only as beta |
| 1.21.7 | tested | NeoForge only as beta |
| 1.21.8 | tested | |
| 1.21.9, 1.21.10, 1.21.11 | port in progress | |
| 26.1 | tested | NeoForge only as beta |
| 26.1.1 | tested | NeoForge only as beta |
| 26.1.2 | tested | |
| 26.2 | tested | |
| 26.3 | tested | NeoForge only as beta |

"NeoForge only as beta" means that NeoForge had no stable build for that Minecraft version on
the test date (2026-09-30). The Pipster jar was tested with that beta.

## Exact versions used in testing

| Minecraft | Fabric Loader | Fabric API | Team Reborn Energy | NeoForge |
|---|---|---|---|---|
| 1.21.1 | 0.19.5 | 0.116.17+1.21.1 | 4.1.0 | 21.1.252 |
| 1.21.2 | 0.19.5 | 0.106.1+1.21.2 | 4.1.0 | 21.2.1-beta |
| 1.21.3 | 0.19.5 | 0.114.1+1.21.3 | 4.1.0 | 21.3.97 |
| 1.21.4 | 0.19.5 | 0.119.4+1.21.4 | 4.1.0 | 21.4.158 |
| 1.21.5 | 0.19.5 | 0.128.2+1.21.5 | 4.2.0 | 21.5.98 |
| 1.21.6 | 0.19.5 | 0.128.2+1.21.6 | 4.2.0 | 21.6.20-beta |
| 1.21.7 | 0.19.5 | 0.129.0+1.21.7 | 4.2.0 | 21.7.25-beta |
| 1.21.8 | 0.19.5 | 0.136.1+1.21.8 | 4.2.0 | 21.8.54 |
| 26.1 | 0.19.5 | 0.145.1+26.1 | 5.0.0 | 26.1.0.19-beta |
| 26.1.1 | 0.19.5 | 0.145.4+26.1.1 | 5.0.0 | 26.1.1.15-beta |
| 26.1.2 | 0.19.5 | 0.155.3+26.1.2 | 5.0.0 | 26.1.2.112 |
| 26.2 | 0.19.5 | 0.161.0+26.2 | 5.0.0 | 26.2.0.88 |
| 26.3 | 0.19.5 | 0.161.0+26.3 | 5.0.0 | 26.3.0.37-beta |

On Fabric, Team Reborn Energy is included inside the Pipster jar (jar-in-jar), so you do not
need to install it separately. Fabric API is required.

Java: 21 for Minecraft 1.21.x, 25 for Minecraft 26.x.

## Moving worlds between versions

* Updating Minecraft (and using the matching Pipster jar) keeps pipes, settings, buffers and
  channels. The save format is the same in all versions.
* Going back to an older Minecraft version is not supported by Minecraft itself.
* Moving a world between Fabric and NeoForge uses the same ids and data, but this has not been
  tested yet.
* Always back up your world before updating.
