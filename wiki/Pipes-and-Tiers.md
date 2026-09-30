# Pipes and Tiers

## The four kinds

| Kind | Moves | Connects to |
|---|---|---|
| Item Pipe | items (with all their components, e.g. enchantments, names) | inventories |
| Fluid Pipe | fluids | fluid tanks and other fluid containers |
| Gas Pipe | gases (see [Resources and Compatibility](Resources-and-Compatibility)) | gas handlers |
| Energy Cable | energy (E = FE = Team Reborn Energy units) | energy storages |

A pipe position holds exactly one kind. Pipes of the same kind connect regardless of tier;
different kinds never connect. The pipe is not a tank you can fill from outside.

## Tiers

| Tier | Items/t | Fluid mB/t | Gas units/t | Energy E/t | Item batch |
|---|---:|---:|---:|---:|---:|
| T1 Copper | 2 | 100 | 100 | 1,000 | 8 |
| T2 Reinforced | 8 | 400 | 400 | 8,000 | 32 |
| T3 Precision | 32 | 1,600 | 1,600 | 64,000 | 64 |
| T4 Resonant | 128 | 6,400 | 6,400 | 512,000 | 256 |
| T5 Quantum | 512 | 25,600 | 25,600 | 4,096,000 | 1,024 |

These are balance values that can still change before 1.0.

### How throughput works

* Every **pipe segment** has one budget per tick. All transfers through that segment share it,
  in both directions.
* Every **port** also has a budget of its own tier.
* So one Copper segment limits a path even if both ends are Quantum ports. Parallel paths can
  add up throughput, but a single port never goes above its own budget.
* Items can save up unused port throughput for up to 4 ticks to move in batches ("Item batch").
  Normal stack sizes still apply, and batches never exceed the current segment budget.
* Gas values are in the unit the gas declares (see
  [Resources and Compatibility](Resources-and-Compatibility)); they are not converted.

### Mixing tiers

You can mix tiers in one network. Each segment is limited by its own tier. Hover over a pipe item
to see its throughput.

## Upgrading

There are two ways to upgrade.

**In place, with a bolt.** Right-click a pipe with the matching upgrade bolt:

| Bolt | Upgrades |
|---|---|
| Reinforcement Upgrade Bolt | T1 → T2 |
| Precision Upgrade Bolt | T2 → T3 |
| Resonance Upgrade Bolt | T3 → T4 |
| Quantum Upgrade Bolt | T4 → T5 |

The pipe keeps its connections, port settings, cards and buffered content.

**By crafting.** 8 pipes of one tier around the next tier's core item give 8 pipes of the next
tier (see [Recipes](Recipes)). Pipes that still carry buffered content or block data do **not**
fit into these recipes, so crafting never deletes resources.

## Breaking pipes

* Break a pipe normally, or sneak + right-click its centre with the Conduit Wrench.
* Filter and rule cards in its ports drop as items.
* Buffered resources stay inside the dropped pipe item and come out again when you place it.
