# Ports and the Conduit Wrench

A **port** is the place where a pipe touches a block that can take or give resources. Ports
appear on their own when you place a pipe next to such a block. All settings and cards belong to
the port, not to a separate filter block.

## The Conduit Wrench

| Action | Effect |
|---|---|
| Right-click a port | open the port screen |
| Sneak + right-click an arm | connect or disconnect exactly that arm (also splits networks) |
| Sneak + right-click the pipe centre | dismantle the pipe |
| Sneak + right-click air | switch to the next of 3 copy profiles |

You need build permission at that spot. The server checks distance and permission for every
action.

## Port settings

### Mode

Directions are always seen **from the network**.

| Mode | Meaning |
|---|---|
| Insert | network → block (default for new ports) |
| Extract | block → network |
| Both | both directions |
| Off | port disabled |

On Fabric, energy generators *push* energy into cables (the usual Fabric energy convention).
A port must be set to **Extract** or **Both** to accept that push.

### Redstone

* **RS: any**: ignore redstone
* **RS: on**: work only with a redstone signal
* **RS: off**: work only without a redstone signal

### Priority

From −99 to 99 (Shift: steps of 10). Targets with higher priority are served first. Lower
priorities only get resources when all higher ones accept nothing right now.

### Distribution

How a source port picks among targets with the same priority:

* **Rotate** (round robin, default): take turns
* **Nearest**: the closest target first
* **Sticky**: keep one target until it is full, gone or filtered out

### Rate limit and energy reserve (Rule Card)

With a [Rule Card](Filter-and-Rule-Cards#rule-card) in the port:

* **Port limit**: maximum amount per tick for this port (Shift: ×10 steps). It can only lower
  the tier's rate, never raise it.
* **Reserve** (energy): amount of energy that always stays in the source.

## The status line

The port screen shows why a port is or is not moving anything, for example:

| Status | Meaning |
|---|---|
| Active | resources are moving |
| Idle | nothing to do |
| Source empty / Targets full / No targets | what it says |
| Filter blocks | a filter rejects everything currently available |
| Redstone blocks | the redstone setting stops the port |
| Budget exhausted | the segment or port budget is used up this tick |
| Network rebuilding | the network changed and is recalculated in bounded steps |
| Deferred (server work limit) | the server-wide work limit for this tick is reached; it continues next tick |
| Quarantined: faulty foreign handler | a block from another mod reported impossible numbers and is paused for a while |
| Quantity rule unavailable for this machine | the block does not report its contents reliably, so the quantity rule is blocked |

## Copy and paste

The port screen has **Copy** and **Paste** buttons. They store the port settings (mode, redstone,
priority, distribution, limits, filter rules) in the wrench's active profile. The wrench holds
3 profiles.

* **Cards are never copied.** Paste applies the settings; if the target port has no card or a
  weaker card, the extra rules are kept but shown as inactive.
* A profile only pastes onto ports of the same transport kind.

## Other players

In multiplayer every change is checked on the server. If two players edit the same port, the
second screen notices the newer settings and refreshes ("Settings changed elsewhere").
