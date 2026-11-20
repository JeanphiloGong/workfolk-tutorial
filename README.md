# Workfolk Handbook

This repository is the from-zero handbook for **Workfolk**, a game-like world
where autonomous agents pursue goals, use shared resources, and adapt to each
other's work.

The handbook and the runnable project are separate repositories:

- [`../workfolk`](../workfolk) contains the Python runtime.
- This repository explains the design, contracts, version checkpoints, and
  verification scenarios used to build that runtime.

The core model is an event-driven autonomous world. A goal can produce many
activities, activities can run in parallel, and new activities can be created
when the world changes. A fixed research-to-writing-to-review pipeline is one
possible arrangement inside the world, not the world model itself.

## Reading Order

1. [`plan.md`](plan.md) explains the complete loop and V0 through V7.
2. [`contracts/world-state.md`](contracts/world-state.md) defines shared facts.
3. [`contracts/agent-model.md`](contracts/agent-model.md) defines autonomous
   units and their private execution loop.
4. [`contracts/interaction.md`](contracts/interaction.md) defines commands,
   long-running activities, and world events.

The current checkpoint is V0 design. It is a reviewable contract baseline and
does not claim that the runtime is implemented.
