# Recipes

All recipes are normal datapack recipes and can be changed with a datapack. Generated from the
recipe files of the mod; blank cells are empty grid slots.

## Materials

### Copper Sleeve ×8

| | | |
|---|---|---|
| Copper Ingot |   | Copper Ingot |
| Copper Ingot |   | Copper Ingot |
| Copper Ingot |   | Copper Ingot |

### Gasket ×4

| | | |
|---|---|---|
|   | Dried Kelp |   |
| Dried Kelp | Slime Ball | Dried Kelp |
|   | Dried Kelp |   |

### Insulation ×4

| | | |
|---|---|---|
| any Wool | any Wool | any Wool |
|   | Dried Kelp |   |
| any Wool | any Wool | any Wool |

### Conductor Core ×4

| | | |
|---|---|---|
| Copper Ingot | Redstone | Copper Ingot |
| Copper Ingot | Redstone | Copper Ingot |
| Copper Ingot | Redstone | Copper Ingot |

### Port Ring ×8

| | | |
|---|---|---|
|   | Iron Ingot |   |
| Iron Ingot |   | Iron Ingot |
|   | Iron Ingot |   |

### Reinforcement Kit ×4

| | | |
|---|---|---|
| Iron Ingot | Iron Ingot | Iron Ingot |
| Gasket | Gasket | Gasket |
| Iron Ingot | Iron Ingot | Iron Ingot |

### Precision Core ×4

| | | |
|---|---|---|
| Gold Ingot | Quartz | Gold Ingot |
| Redstone | Comparator | Redstone |
| Gold Ingot | Quartz | Gold Ingot |

### Resonance Core ×4

| | | |
|---|---|---|
| Amethyst Shard | Ender Pearl | Amethyst Shard |
| Quartz | Precision Core | Quartz |
| Amethyst Shard | Ender Pearl | Amethyst Shard |

### Quantum Core ×4

| | | |
|---|---|---|
| Diamond | Netherite Scrap | Diamond |
| Ender Eye | Resonance Core | Ender Eye |
| Diamond | Netherite Scrap | Diamond |

## Tools and cards

### Conduit Wrench ×1

| | | |
|---|---|---|
| Iron Ingot |   | Iron Ingot |
|   | Redstone |   |
|   | Stick |   |

### Basic Filter Card ×1

| | | |
|---|---|---|
| Iron Nugget | Paper | Iron Nugget |
|   | Redstone |   |
| Iron Nugget | Paper | Iron Nugget |

### Advanced Filter Card ×1

| | | |
|---|---|---|
| Gold Nugget | Quartz | Gold Nugget |
|   | Basic Filter Card |   |
| Gold Nugget | Comparator | Gold Nugget |

### Precision Filter Card ×1

| | | |
|---|---|---|
| Diamond | Amethyst Shard | Diamond |
| Quartz | Advanced Filter Card | Quartz |
| Diamond | Comparator | Diamond |

### Rule Card ×1

| | | |
|---|---|---|
| Port Ring | Paper | Port Ring |
| Redstone | Comparator | Redstone |
| Port Ring | Paper | Port Ring |

## Tier 1 pipes

### Copper Item Pipe ×8

| | | |
|---|---|---|
| Copper Sleeve | Copper Sleeve | Copper Sleeve |
| Glass | Chest | Glass |
| Copper Sleeve | Copper Sleeve | Copper Sleeve |

### Copper Fluid Pipe ×8

| | | |
|---|---|---|
| Copper Sleeve | Copper Sleeve | Copper Sleeve |
| Glass | Gasket | Glass |
| Copper Sleeve | Copper Sleeve | Copper Sleeve |

### Copper Gas Pipe ×8

| | | |
|---|---|---|
| Copper Sleeve | Copper Sleeve | Copper Sleeve |
| Gasket | Iron Ingot | Gasket |
| Copper Sleeve | Copper Sleeve | Copper Sleeve |

### Copper Energy Cable ×8

| | | |
|---|---|---|
| Insulation | Insulation | Insulation |
| Conductor Core | Conductor Core | Conductor Core |
| Insulation | Insulation | Insulation |

## Upgrade bolts

### Reinforcement Upgrade Bolt ×8

Shapeless: Reinforcement Kit

### Precision Upgrade Bolt ×8

Shapeless: Precision Core

### Resonance Upgrade Bolt ×8

Shapeless: Resonance Core

### Quantum Upgrade Bolt ×8

Shapeless: Quantum Core

## Tesseracts

### Resonant Tesseract ×1

| | | |
|---|---|---|
| Ender Eye | Resonance Core | Ender Eye |
| Resonance Core | Ender Chest | Resonance Core |
| Ender Eye | Resonance Core | Ender Eye |

### Quantum Tesseract ×1

| | | |
|---|---|---|
| Diamond | Netherite Ingot | Diamond |
| Quantum Core | Resonant Tesseract | Quantum Core |
| Diamond | Netherite Ingot | Diamond |

## Tier upgrades by crafting

8 pipes of one tier around the core of the next tier give 8 pipes of the next tier:

| | | |
|---|---|---|
| pipe | pipe | pipe |
| pipe | core | pipe |
| pipe | pipe | pipe |

| Result | Pipes | Core |
|---|---|---|
| Reinforced Energy Cable ×8 | Copper Energy Cable | Reinforcement Kit |
| Precision Energy Cable ×8 | Reinforced Energy Cable | Precision Core |
| Resonant Energy Cable ×8 | Precision Energy Cable | Resonance Core |
| Quantum Energy Cable ×8 | Resonant Energy Cable | Quantum Core |
| Reinforced Fluid Pipe ×8 | Copper Fluid Pipe | Reinforcement Kit |
| Precision Fluid Pipe ×8 | Reinforced Fluid Pipe | Precision Core |
| Resonant Fluid Pipe ×8 | Precision Fluid Pipe | Resonance Core |
| Quantum Fluid Pipe ×8 | Resonant Fluid Pipe | Quantum Core |
| Reinforced Gas Pipe ×8 | Copper Gas Pipe | Reinforcement Kit |
| Precision Gas Pipe ×8 | Reinforced Gas Pipe | Precision Core |
| Resonant Gas Pipe ×8 | Precision Gas Pipe | Resonance Core |
| Quantum Gas Pipe ×8 | Resonant Gas Pipe | Quantum Core |
| Reinforced Item Pipe ×8 | Copper Item Pipe | Reinforcement Kit |
| Precision Item Pipe ×8 | Reinforced Item Pipe | Precision Core |
| Resonant Item Pipe ×8 | Precision Item Pipe | Resonance Core |
| Quantum Item Pipe ×8 | Resonant Item Pipe | Quantum Core |

These recipes only accept **empty** pipes: pipes that still carry buffered resources or block data
do not fit, so crafting never deletes resources. To upgrade a pipe that is already placed, use an
upgrade bolt instead (see [Pipes and Tiers](Pipes-and-Tiers)).
