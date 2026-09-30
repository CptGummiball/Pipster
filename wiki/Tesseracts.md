# Tesseracts

A tesseract sends resources through a **channel** to other tesseracts on the same channel, over
any distance and without pipes in between.

| Block | Range | Throughput per resource type |
|---|---|---|
| Resonant Tesseract | any distance in the **same dimension** | T4 (Resonant) |
| Quantum Tesseract | across **all dimensions** of the server | T5 (Quantum) |

A channel between a Resonant and a Quantum Tesseract works only within one dimension. Only if
both are Quantum Tesseracts can it cross dimensions.

## Channels

Right-click a tesseract to open the channel screen.

* Every tesseract can bind **one channel per resource type** (items, fluids, gas, energy).
* **Create** a channel with a name. It belongs to you.
* **Visibility**:
  * **Private**: only you
  * **Invited players**: you and the players you invite by name
  * **Public**: everyone on the server
* **Delete** removes a channel you own. Tesseracts bound to it go offline.
* A player can own up to 128 channels.

If someone loses access to a channel (visibility changed, invitation removed), their tesseract
stays bound but goes **offline** ("Access revoked: offline"). Nothing is deleted.

## Sides

Each of the six sides of a tesseract works like a pipe port: click a side to change its mode
(Insert, Extract, Both, Off); **Cfg** opens its filter and rule settings. A side can connect to a
machine directly or to a Pipster pipe network.

## Rules that keep things fair

* **All involved chunks must be loaded.** Tesseracts never load chunks. A tesseract in an
  unloaded chunk simply counts as offline.
* **At most one channel hop.** A resource that arrived through a channel cannot enter another
  channel on the same trip. This rules out endless loops.
* Resources never go back into the network they came from.
* A channel is only as fast as its members: the channel budget is the best tier among loaded
  member tesseracts, and each tesseract is still limited by its own tier.
* Two tesseracts placed side by side do not connect to each other, so channels cannot be
  chained that way.

## Breaking a tesseract

* Buffered resources stay in the dropped item and come back when placed.
* The channel binding is **not** kept in the item, so access control cannot be bypassed by
  handing a tesseract to someone else.

## Server operators

Operators (permission level 2) can manage channels:

```
/pipster channels list
/pipster channels delete <channel-uuid>
```
