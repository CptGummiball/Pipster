# Getting Started

This page moves items from one chest to another. The same steps work for fluids (fluid pipes
between tanks or cauldrons), gas and energy.

## 1. Craft the basics

You need copper, iron, glass, redstone and a chest. See [Recipes](Recipes) for the patterns.

* **Copper Sleeve** (8 from 6 copper ingots)
* **Copper Item Pipe** (8 from 6 copper sleeves, 2 glass and a chest)
* **Conduit Wrench** (2 iron ingots, redstone, stick)

## 2. Place the pipes

Place two chests and connect them with a line of Copper Item Pipes.

* Pipes of the same kind connect to each other, whatever their tier.
* Different kinds (item, fluid, gas, energy) never connect.
* A pipe connects to a machine or container only if that block offers the right interface on
  that side. The connection gets a thicker **port** (a flange) where the pipe meets the block.

## 3. Set one port to Extract

Every new port starts as **Insert**: the network delivers into the block. Nothing is pulled out
yet, so nothing moves.

Right-click the port at the source chest with the **Conduit Wrench**. In the port screen, click
the mode button until it says **Extract**. Items now flow from that chest into the network and
from there into every port set to Insert.

The screen always describes directions as seen from the network:

* **Insert**: network → block
* **Extract**: block → network
* **Both**: both directions
* **Off**: port disabled

## 4. What to try next

* Put a **Basic Filter Card** into the port to let only certain items through, see
  [Filter and Rule Cards](Filter-and-Rule-Cards).
* Use **priority** to fill one chest first, see
  [Ports and the Conduit Wrench](Ports-and-the-Conduit-Wrench).
* Upgrade the line with a **Reinforcement Upgrade Bolt**: right-click a Copper pipe with it, see
  [Pipes and Tiers](Pipes-and-Tiers).
* Send items somewhere far away with a [Tesseract](Tesseracts).

## Things that are good to know

* Throughput is limited **per pipe segment**. One slow segment limits the whole path.
* Pipes can hold a small buffer of resources on the way. If you break a pipe with content in it,
  the dropped pipe item keeps the content (tooltip: "Contains buffered resources"). Place it
  again to release it. Such pipes cannot be used in upgrade recipes, so nothing is lost by
  crafting.
* Pipes only work in loaded chunks. Pipster never loads chunks on its own.
