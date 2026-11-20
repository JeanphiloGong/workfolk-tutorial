# Social Adapter Module

## Mode and owner

This is a backend adapter module. Workfolk owns coordination mechanics. The
social-media application owns its Brief, Research, Draft, Review, authorization,
and publishing contracts.

## Bounded flow

The adapter receives a validated social brief and creates a Workfolk Goal. It
can propose research and writing Activities, binding a writing Activity to the
research Artifact ID. It does not perform a browser operation or manufacture a
review result.

The implementation lives in `workfolk/src/workfolk/adapters/social-media.ts`.
Its mapping check is `workfolk/tests/test_v7_social_adapter.mjs`, described in
[`../versions/v7-social-adapter.md`](../versions/v7-social-adapter.md).
