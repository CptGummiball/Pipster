# Performance

## How Pipster keeps servers fast

* **No work per pipe.** Pipes do not tick. The server keeps one transport graph per dimension
  and resource type and only works on ports that have something to do.
* **Bounded work per tick.** Network rebuilds, route searches and machine accesses all have a
  per-tick limit (see [Configuration](Configuration)). Big changes are spread over several
  ticks instead of freezing the server.
* **Idle ports back off.** A port that finds nothing to move waits longer and longer before it
  tries again (up to about 3 seconds), with a little randomness so ports do not all wake up in
  the same tick.
* **No chunk loading.** Pipster never loads chunks and never scans the world.
* **Nothing is simulated on the client.** Clients only render pipe shapes.

## Measured numbers

Measured on 2026-09-30 on one machine: Intel Core i7-13700H (20 threads), 32 GB RAM,
Windows 11, Java 21, dedicated server, Minecraft 1.21.1. Each scene ran 3 times: 1 minute
warm-up, then 5 minutes (6000 ticks) of measurement. "p95" is the time per tick that 95 % of
ticks stayed under.

| Scene | Pipster time per tick, p95 (Fabric) | (NeoForge) | Whole server tick, p95 |
|---|---|---|---|
| 10,000 idle pipes | about 0.01 ms | about 0.01 ms | 0.3 – 0.6 ms |
| 10,000 pipes, 100 active ports moving items | 1.33 – 1.38 ms | 0.81 – 1.05 ms | 1.2 – 1.9 ms |
| 50,000 pipes, 500 active ports moving items | 4.95 – 5.16 ms | 4.40 – 4.59 ms | 4.9 – 5.7 ms |
| a network of 6,400 pipes split in two | about 0.01 ms | about 0.01 ms | 0.3 – 0.5 ms |

A Minecraft tick has a budget of 50 ms.

**These numbers apply only to this machine and these scenes.** Other hardware, other mods,
slow machines at the ends of pipes and very large bases change them. They are not a promise of
20 TPS.

How it was measured: the scenes are built automatically on a fresh dedicated server. Pipster's
own time is measured around its tick work; the whole server tick is measured from tick start to
tick end. The first minute is discarded as warm-up. No other mods are installed.

## Tips for large bases

* Prefer a few busy ports over many ports that rarely have work.
* Use higher tiers instead of many parallel low-tier lines.
* Filters are cheap, but hundreds of rules on every port add up.
* If a network shows "Network too large", split it with the wrench (sneak + right-click an arm).
