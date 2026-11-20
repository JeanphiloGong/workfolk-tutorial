# Workfolk From-Zero Plan

## Purpose

Workfolk is a small learning project for understanding autonomous multi-agent
coordination through a playable shared world. The player can set a goal,
observe units working, intervene, and judge the resulting work. Agents can also
make progress without a player assigning every intermediate step.

The game-like reference is the cooperative pressure of *Overcooked*. Its
useful idea is a shared environment where several decision-makers affect the
same resources and see the consequences. Workfolk adds autonomous work loops,
long-running tools, durable artifacts, and human intervention because those
are required for an agent office.

## Complete Logic

```text
Player or supervisor creates a Goal and constraints
  -> agents observe the current WorldState
  -> an agent selects or proposes an Activity it can perform
  -> the agent runs its private observe-decide-execute loop
  -> the environment checks shared resources and records effects
  -> an Artifact, Message, or failure Event is published
  -> affected agents and the supervisor observe the event
  -> activities continue, change, or produce new activities
  -> the goal is accepted, revised, paused, or left incomplete
```

The world advances on completed commands and external results. It does not
require every Agent to submit a synchronous movement action on every clock
tick. A long model or browser operation is an Activity with an Execution. The
world records its visible lifecycle while the Agent owns its private execution
details.

## Core Concepts

| Concept | Responsibility |
| --- | --- |
| `WorldState` | Shared goals, agents, activities, resources, artifacts, messages, and current facts |
| `Goal` | Desired outcome, constraints, budget, and acceptance conditions |
| `Agent` | Stable actor with capabilities, a controller, private context, and current activities |
| `Activity` | A piece of work an Agent is currently pursuing or proposing |
| `Action` | A request to observe, reserve, start, deliver, ask, release, or complete something |
| `Execution` | One actual run of an Activity, including external calls and result status |
| `Resource` | A shared account, browser slot, tool, file, or other limited capability |
| `Artifact` | A versioned output that another Agent or the player can consume |
| `Event` | A confirmed fact that changes what other participants can observe |
| `Message` | A communication request or response linked to an Agent and context |

`Task` may be used as a user-facing assignment or work contract. It is not the
Agent's complete behavior and it does not impose a universal stage sequence.

## V0 Design and Contracts

V0 delivers the complete logic, boundaries, and contracts before runtime code.
The initial documents are:

- [`contracts/world-state.md`](contracts/world-state.md)
- [`contracts/agent-model.md`](contracts/agent-model.md)
- [`contracts/interaction.md`](contracts/interaction.md)

V0 decisions include:

- Agents have private execution context and observable shared state.
- Activities can be created from a goal, an Agent proposal, or a supervisor
  decision.
- The environment is the only writer of shared state.
- Shared resource reservation and artifact publication are explicit events.
- Long-running work is asynchronous at the world boundary.
- A player can intervene without pretending that an animation or message means
  a real execution completed.
- The first runtime uses deterministic fake controllers and tools with no
  external side effects. The runtime uses the Pi Agent framework at its
  controller and tool boundary, while world rules remain independent of model
  calls.

V0 acceptance means the contracts explain ownership, valid transitions,
failure paths, and the next implementation checks. It does not mean runtime
behavior exists.

## Version Line

| Version | Starts from | Adds | Excludes | Acceptance |
| --- | --- | --- | --- | --- |
| V1 | V0 contracts | One Agent, one Goal, one fake tool, and a private execution loop | Multiple agents, UI, external tools | A fixed input produces a deterministic Artifact and Event sequence |
| V2 | V1 runtime | Two autonomous Agents sharing a resource and observing each other's events | LLMs, browsers, persistent database | One Agent waits or changes activity when the resource is occupied; the other continues |
| V3 | V2 runtime | Dynamic activities, optional dependencies, and a supervisor Agent | Human UI, real publishing | A result can open a new activity while an independent activity continues |
| V4 | V3 runtime | Durable event journal, execution history, and deterministic replay | Automatic recovery of unknown external side effects | Replaying the journal reconstructs the same shared facts and artifacts |
| V5 | V4 runtime | Real tool adapter and Pi-backed controller behind the same Agent contract | Social-platform publishing | A real tool result enters the world only through a verified event and artifact |
| V6 | V5 runtime | Read-only replay viewer with play, pause, step, reset, and Agent/event projection | Live mutation and multiplayer deployment | A player can inspect the same frames repeatedly without changing world state |
| V7 | V6 runtime | Social-media adapter for research and writing Activities | Making social-media the runtime's domain model or publishing real content | A social brief maps to a Workfolk Goal and its Artifacts keep their own contract |

## First Implementation Check

The first executable scenario is deliberately small:

```text
Goal: produce a short structured briefing from a supplied source
Agent: one autonomous research worker
Tool: deterministic local source reader
Result: one versioned briefing artifact
```

The test must show the Agent choosing its activity, executing it, publishing an
artifact, and causing a world event. A caller that directly inserts the
artifact without running the Agent loop does not satisfy V1.

## V0 Implementation Handoff

The current runtime baseline is the sibling repository
`../workfolk`. It contains `package.json`, the TypeScript package at
`src/workfolk/index.ts`, and no domain behavior. V0 therefore ends with
design readiness. It does not claim that the first scenario already runs.

The backend module owns the world rules and the shared contracts. Proposed
implementation targets are relative to `workfolk/`:

| Version | Runtime targets | Reader check |
| --- | --- | --- |
| V1 | `src/workfolk/model.ts`, `src/workfolk/world.ts`, `src/workfolk/agents.ts`, `tests/test_v1_single_agent.mjs` | A scripted Agent selects one Activity, runs the deterministic source tool, publishes one Artifact, and emits the expected events |
| V2 | `src/workfolk/resources.ts`, `src/workfolk/world.ts`, `src/workfolk/agents.ts`, `tests/test_v2_shared_resource.mjs` | Two Agents observe one exclusive resource; the losing Agent waits or changes Activity while the other continues |
| V3 | `src/workfolk/supervisor.ts`, `src/workfolk/activities.ts`, `tests/test_v3_dynamic_activities.mjs` | An Artifact event opens a new Activity while an unrelated Activity remains runnable |
| V4 | `src/workfolk/journal.ts`, `src/workfolk/replay.ts`, `tests/test_v4_replay.mjs` | A saved event journal reconstructs the same state and Artifact versions in a separate process |
| V5 | `src/workfolk/tools.ts`, `src/workfolk/controllers.ts`, `src/workfolk/pi-controller.ts`, `tests/test_v5_tool_boundary.mjs` | A Pi-backed controller can request a tool, but only a verified tool result can publish a world event |
| V6 | `viewer/index.html`, `viewer/app.js`, `viewer/replay-state.js`, `viewer/styles.css`, `tests/test_v6_viewer.mjs` | A replay can be played, stepped, and reset without changing its frames |
| V7 | `src/workfolk/adapters/social-media.ts`, `tests/test_v7_social_adapter.mjs` | A social brief produces a Goal and input-bound research/writing Activities |

Each version keeps the previous version's public model and event semantics. The
runtime may change its internal storage when a version requires it, but the
viewer and later controllers consume the same shared-world contract.

The viewer is a linked frontend module that consumes `contracts/viewer.md` and
the backend event contract. It begins as a read-only replay projection. Live
player actions and artifact editing require a later frontend design checkpoint.
The social-media adapter maps its own Brief, Research, Draft, Review, and
publishing records into Workfolk Activities; it does not replace those domain
contracts.

## V0 Freeze

V0 is ready for the implementation tutorial when the three contract documents
and this plan agree on Agent ownership, Activity lifecycle, resource
reservation, Artifact immutability, asynchronous Execution results, failure
events, and the V1 check above. The initial world implementation remains free
of browser, database, and network effects. The runtime package records the
existing Pi dependency so the V5 controller can use the established framework.
A later implementation checkpoint may invoke Pi or another external tool only
when its version includes a concrete reader check.

## Boundary With Social Media

Workfolk owns coordination mechanics. The `social-media` project owns its
content rules, strategy selection, review evidence, browser operations, account
authorization, and publishing records. An adapter may translate a social goal
into Workfolk activities after the generic runtime is verified.
