# Configuration

Server settings live in `config/pipster-server.properties` in the server directory. On a
singleplayer world, the integrated server uses the game directory's `config` folder. The file is
created with the defaults on the first start. Clients never receive this file.

The defaults are starting values. They are safe for most servers, but they are not performance
promises. Measure before and after you change them.

| Key | Default | Range | What it does |
|---|---:|---|---|
| `rebuildVisitsPerTick` | 4096 | 16 – 1,048,576 | How many pipe nodes a network rebuild may visit per tick. Large networks rebuild over several ticks. |
| `maxNetworkSize` | 65536 | 16 – 16,777,216 | Networks larger than this stop working and show "Network too large". |
| `syncMergeLimit` | 4096 | 0 – 1,048,576 | Up to this size, joining two networks is done immediately instead of spread over ticks. |
| `routeCacheEntries` | 1024 | 16 – 65,536 | Cached routes per network. |
| `routeVisitsPerTick` | 65536 | 64 – 16,777,216 | Work limit for finding new routes per tick. |
| `endpointAttemptsPerDimension` | 2048 | 16 – 1,048,576 | How many machine accesses one dimension may make per tick. |
| `slotVisitsPerDimension` | 32768 | 64 – 16,777,216 | How many inventory slots one dimension may look at per tick. |
| `endpointAttemptsServer` | 8192 | 16 – 4,194,304 | The same limit for the whole server. |
| `slotVisitsServer` | 131072 | 64 – 67,108,864 | The same limit for the whole server. |
| `portRefreshesPerTick` | 256 | 1 – 65,536 | How many ports may re-check their neighbour block per tick after changes. |
| `quarantineTicks` | 1200 | 20 – 72,000 | How long a misbehaving foreign block is paused (1200 ticks = 1 minute). |
| `acceptForeignPush` | true | true/false | Whether Extract/Both ports accept items, fluids and gas that other blocks push in (e.g. machines with auto-output). Energy pushes are always accepted. |

When a work limit is reached, ports show "Deferred (server work limit)" and continue the next
tick. Nothing is lost.

## Datapack options

* Fluid tag `#pipster:gaseous`: fluids in this tag travel in gas pipes, see
  [Resources and Compatibility](Resources-and-Compatibility).
* All recipes can be replaced with a datapack. Upgrade recipes use the recipe type
  `pipster:empty_contents_shaped`, which works like a normal shaped recipe but refuses pipes that
  still carry content.
