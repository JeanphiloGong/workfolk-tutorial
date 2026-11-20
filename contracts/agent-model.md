# Agent Model Contract

An Agent is an autonomous actor with a stable identity and its own execution
loop. It can receive assignments, notice available work, propose an Activity,
use permitted tools, and change its plan after observing an Event.

## Agent Fields

| Field | Meaning |
| --- | --- |
| `id` | Stable identity across Activities and Executions |
| `capabilities` | Operations and tool classes the Agent may use |
| `controller` | Human, scripted, supervisor, or model-backed decision policy |
| `private_context` | Current plan, conversation context, and intermediate data |
| `current_activity` | The visible Activity currently being pursued |
| `status` | `idle`, `deciding`, `working`, `waiting`, `blocked`, or `offline` |
| `budget_scope` | Limits available to this Agent |

## Private Loop

```text
observe(WorldState snapshot)
  -> select or revise an Activity
  -> request an allowed Action
  -> run tools or wait for an external result
  -> report an Artifact, Message, failure, or request
  -> update private context
  -> observe the next relevant Event
```

The Agent can carry out several internal tool steps during one Activity. The
world sees the Activity lifecycle and its shared effects. A controller cannot
write another Agent's state, create an Artifact without an accepted result, or
claim a resource it has not reserved.

## Controllers

The same Agent contract supports different controllers:

- a human controller supplies actions through the viewer;
- a scripted controller makes deterministic decisions for tests;
- a supervisor controller proposes goals and Activities;
- a model-backed controller chooses actions through an SDK.

Changing a controller does not change the shared-world rules.
