---
name: tidy-effector
description: Plan, implement, and review small behavior-preserving Effector and effector-react model changes using Kent Beck's empirical software-design method and current Effector unit, sample, Scope, useUnit, and testing guidance. Use for event-store-effect graphs, sample chains, scopes, factories, React bindings, async flows, or Effector model API migrations.
---

# Tidy Effector

Reshape the reactive graph only enough to make the requested event-to-result behavior safe and explicit. Treat events, stores, effects, and their connections as the structure under design.

## Establish the graph behavior

1. Read repository instructions and inspect installed Effector versions, model modules, React bindings, scopes, tests, and package scripts.
2. Trace the relevant path: entry event -> sampled state -> transformation -> effect or target -> resulting store/event.
3. State the requested observable result and the evidence that proves it.
4. Run the narrowest useful baseline test when practical and record pre-existing failures.

Classify each step:

- `S` - rewires or renames units while preserving the same accepted events, effects, state transitions, and public model contract.
- `B` - changes an accepted event, effect invocation, state transition, error path, concurrency outcome, or public model contract.

## Choose when to tidy

State one decision:

- `before` - a small known graph change immediately localizes or clarifies the requested behavior.
- `after` - executing the behavior reveals the correct event or state boundary.
- `later` - useful graph cleanup is larger than this behavior change.
- `never` - the graph is stable or cleanup creates no valuable option.

Avoid speculative model architecture. Build abstractions from observed repeated flows or verified co-change.

## Execute a reversible sequence

1. Propose the shortest `S -> scope test -> B -> scope test` sequence.
2. Apply one small graph change at a time.
3. Test through a fresh `Scope` after every meaningful structural step.
4. Implement the requested behavior as a distinct logical change.
5. Stop when the event-to-result path is verified.

Do not create commits unless asked. If commits are requested, keep graph-only and behavioral changes separately reviewable.

## Select Effector moves

Read [references/guide.md](references/guide.md) before changing unit connections, async work, scopes, or React bindings. Prefer:

- events as facts or entry points;
- stores as state, updated immutably;
- effects for work that can succeed, fail, throw, or touch external systems;
- pure `sample` filters and transformations for declarative connections;
- `sample` over `getState()` in business logic;
- `watch` for debugging or integration, not business logic;
- `useUnit` for React reads and calls;
- `Scope`, `fork`, and `allSettled` for isolated tests;
- `scopeBind` when external async callbacks must re-enter the graph;
- a compatibility event or adapter model for gradual API migration;
- factories only for genuinely repeated model structure;
- temporary consolidation when scattered units hide one business flow.

Do not call units from pure computations or hide ordering in imperative callbacks. Check the installed version and current official docs before using version-sensitive APIs.

## Analyze coupling precisely

Name the units and the change class:

`Changing <event payload/store shape/effect contract> requires changing <other units or consumers>.`

Use `sample` edges, `.on` subscriptions, React consumers, serialization, and co-change history as evidence. Do not equate any graph edge with harmful coupling.

## Verify and report

Use repository-native typecheck, lint, tests, and SSR/client checks where applicable. Prefer isolated scope tests with mocked effect handlers and explicit final state or event assertions.

Report:

- requested event-to-result behavior and proof;
- timing decision;
- graph-only moves and why they were required;
- behavioral change;
- checks run and results;
- scope or concurrency risks and one bounded deferred item.

