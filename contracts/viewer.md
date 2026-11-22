# Viewer Contract

The Workfolk viewer displays a replay or live World snapshot and sends explicit
player commands through the local runtime boundary. It owns the display frame
and pending UI state. The runtime owns World rules and all state changes.

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
- The pointer changes only the projection.
- A live message, pause request, or reassignment is submitted to POST /actions
  and remains pending until the response contains a confirmed Transition or an
  explicit rejection.
- GET /world returns the World snapshot and recent Events. A stale response
  cannot overwrite a newer revision.
- The viewer must not infer completion from animation timing.

## Local HTTP Contract

GET /world returns a World snapshot and recent events.

POST /actions accepts one player Action and returns the resulting Transition or
a rejection. The server owns the active World instance. Browser code does not
import or duplicate the runtime domain rules.
