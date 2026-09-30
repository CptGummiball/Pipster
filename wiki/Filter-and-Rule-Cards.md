# Filter and Rule Cards

Pipster has no filter block. Instead, every port has two card slots: one for a **filter card**
and one for a **Rule Card**. The card unlocks features; the rules themselves are saved in the
port.

| Card | Rules | Adds |
|---|---:|---|
| Basic Filter Card | 9 | exact item/fluid/gas id, allowlist or denylist |
| Advanced Filter Card | 27 | tags, mod namespace, exact component comparison |
| Precision Filter Card | 54 | quantity rules: "keep N in the source", "fill the target up to N" |
| Rule Card | – | port rate limit, energy reserve |

Filters work on every pipe tier. A better card does not make the pipe faster.

## Setting up rules

1. Put a filter card into the filter slot of the port screen.
2. Click a rule slot with an item (or a bucket/container for fluids) to set it as a pattern.
   This is a **ghost pattern**: the item is not used up. Right-click clears the slot.
3. Switch between **Allowlist** and **Denylist**.

With an Advanced or Precision card you can also:

* **#Tag**: type a tag into the text field and press #Tag to turn the selected rule into a tag
  rule (e.g. `c:ingots`).
* **Mod**: type a mod id and press Mod to match everything from that mod.
* **Shift-click** a pattern slot: also compare the exact components (for example enchantments
  or a custom name).
* **Qty** (Precision card): cycle the quantity rule of the selected rule; the amount comes from
  the text field.

## How rules are evaluated

* Inside one rule, all conditions must match (AND). Between rules, one match is enough (OR).
* An **empty allowlist blocks everything**. An **empty denylist lets everything through**.
* If an id or tag does not exist (for example, a mod was removed), the rule is kept, marked
  red and matches nothing. It becomes valid again if the mod comes back.
* The source port and the target port must both allow a resource for it to move.
* Tags are recalculated when datapacks reload.

## Quantity rules

* **Keep N in the source**: never take the source below N of this resource.
* **Fill the target up to N**: stop delivering once the target holds N.

Quantity rules need blocks that report their contents reliably. If a block cannot, the port
shows "Quantity rule unavailable for this machine" and the rule blocks instead of guessing.

## Rule Card

The Rule Card unlocks:

* **Port limit**: a maximum rate per tick for this port, below the tier rate.
* **Reserve** (energy ports): energy that always stays in the source.

Energy has no filter card rules (there is nothing to tell apart). Use the Rule Card instead.

## Cards stay cards

* Cards are real items in the port. They drop when the pipe is broken.
* They cannot be inserted or extracted by hoppers or other automation.
* Copy/paste with the wrench copies settings, never cards.
