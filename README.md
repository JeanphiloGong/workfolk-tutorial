# Workfolk Handbook

This repository is the from-zero handbook for **Workfolk**, a game-like world
where autonomous agents pursue goals, use shared resources, and adapt to each
other's work.

The handbook and the runnable project are separate repositories:

- [`../workfolk`](../workfolk) contains the TypeScript runtime and Pi
  integration.
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
5. [`contracts/viewer.md`](contracts/viewer.md) defines the read-only replay
   projection used by the V6 viewer.
6. Read [`versions/v1-single-agent.md`](versions/v1-single-agent.md) through
   [`versions/v7-social-adapter.md`](versions/v7-social-adapter.md) while
   running the matching checks in `workfolk`.
7. The frontend and domain-specific boundaries are described in
   [`modules/viewer.md`](modules/viewer.md) and
   [`modules/social-adapter.md`](modules/social-adapter.md).

The V0 design is the contract baseline. V1 through V7 now have implementation
chapters and matching runtime checkpoints in the sibling repository. The
social-media adapter remains a pure mapping layer and does not perform real
platform operations.
