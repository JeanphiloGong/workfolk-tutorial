# Viewer Module

## Mode and owner

This is the frontend module for the Workfolk replay viewer. The runtime owns
WorldState, events, and replay facts. The viewer owns the current display frame,
playback pointer, and accessible controls.

## Bounded flow

The player opens a replay, sees the first frame, and chooses play, step, or
reset. Each operation changes the pointer and rerenders the same source frames.
An empty or malformed replay stops before rendering. The viewer does not call a
domain operation and cannot turn a displayed animation into a completion fact.

The shared interaction contract is [`../contracts/viewer.md`](../contracts/viewer.md).
The first implementation lives in `workfolk/viewer/` and is checked by
`workfolk/tests/test_v6_viewer.mjs` plus the static HTTP smoke check described in
[`../versions/v6-replay-viewer.md`](../versions/v6-replay-viewer.md).
