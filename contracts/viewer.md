# Viewer Contract

The Workfolk viewer is a read-only projection of a replay or live world
snapshot. It displays the latest shared facts and event history without owning
world rules or changing an Agent's state.

## Replay Input

The first viewer checkpoint consumes a JSON object with ordered `frames`:

| Field | Type | Meaning |
| --- | --- | --- |
| `tick` | integer | Position in the displayed replay |
| `agents` | array | Visible name, activity, and status for each Agent |
| `events` | string array | Human-readable events confirmed at this frame |

The viewer clamps requests outside the available frame range. An empty replay
is an invalid input and must be reported before rendering.

## Interaction Rules

- `Play` advances one frame at a time until the last frame.
- `Step` advances one frame and stops at the last frame.
- `Reset` returns the pointer to frame zero.
- The pointer changes only the projection. It cannot call a runtime operation.
- A later live view may display pending, stale, or failed states from the
  runtime event contract. It must not infer completion from animation timing.
