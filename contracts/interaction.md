# Interaction Contract

Workfolk uses explicit operations and confirmed events. An operation may start
an asynchronous Execution. The result is published only after the operation
boundary verifies what happened.

## Action Families

| Action | Effect |
| --- | --- |
| `propose_activity` | Adds an Activity proposal for a Goal |
| `reserve_resource` | Requests an exclusive or shared reservation |
| `start_execution` | Starts one execution for an Activity |
| `publish_artifact` | Publishes a new immutable Artifact version |
| `send_message` | Adds a message addressed to an Agent or supervisor |
| `release_resource` | Ends a reservation owned by the caller |
| `pause_activity` | Requests a pause at a supported execution boundary |
| `complete_activity` | Requests completion after its acceptance conditions hold |

The environment checks the caller, capability, Activity state, resource
availability, and required inputs before applying an action.

## Events

Events are facts such as:

```text
activity_proposed
resource_reserved
execution_started
artifact_published
message_sent
execution_failed
activity_completed
resource_released
```

An event contains the world revision, actor, affected object, input versions,
and the minimum result data needed by another Agent. It does not contain a
model's unverified statement as proof of completion.

## Long-Running Work

`start_execution` creates a running Execution and returns control to the world.
The Agent can continue working while other Activities progress. A later
`execution_succeeded`, `execution_failed`, `execution_cancelled`, or
`execution_needs_inspection` result closes that execution and emits the next
event. This lets agents work independently without forcing a synchronous Tick
for every tool call.

## Failure Rules

- A denied action leaves shared state unchanged and records the reason.
- A lost external result is `needs_inspection` until evidence resolves it.
- A failed Activity may be revised or replaced while its history remains
  available.
- A player or supervisor instruction changes future decisions only after the
  world records its acceptance and effective revision.
