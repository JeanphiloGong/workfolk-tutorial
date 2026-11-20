# World State Contract

`WorldState` is the shared, observable state of a Workfolk world. Agents read a
snapshot before choosing an activity. Only the environment applies accepted
actions and publishes the resulting state and events.

## Shared Fields

| Field | Meaning |
| --- | --- |
| `world_id` | Stable identifier for one world |
| `revision` | Monotonic version of the shared state |
| `goals` | Desired outcomes and acceptance conditions |
| `agents` | Stable units, capabilities, current activity, and visible status |
| `activities` | Proposed, running, waiting, completed, or failed work |
| `executions` | Actual runs attached to activities |
| `resources` | Shared resources and current reservations |
| `artifacts` | Versioned outputs with producer and input references |
| `messages` | Communication facts and consumption status |
| `events` | Confirmed changes since the requested observation point |
| `budget` | Current limits and consumed amounts |

## Agent Visibility

The shared snapshot may contain an Agent's visible intent, current Activity,
resource reservations, recent result, and failure. It does not contain private
model reasoning, credentials, or an unverified claim that work has completed.

## State Rules

- A state revision changes only through an accepted environment operation.
- A resource has at most one active reservation for a conflicting capability.
- An Artifact is immutable after publication. Editing creates a new version.
- An Activity can have multiple historical Executions, with at most one active
  Execution for a given Activity.
- An Event describes a confirmed fact. A requested action is not itself a fact.
- A Goal can remain open while independent Activities continue.
