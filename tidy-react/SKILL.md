---
name: tidy-react
description: Plan, implement, and review small behavior-preserving React structure changes using Kent Beck's empirical software-design method and current React state, purity, event, and Effect guidance. Use for React component, hook, props, state, Effect, rendering, interaction, or component-boundary changes in .jsx or .tsx code.
---

# Tidy React

Reshape React code only enough to make the requested user-visible change safe and easy. Preserve interaction semantics, rendered output, accessibility, and external synchronization during structural steps.

## Establish the UI behavior

1. Read repository instructions and inspect the affected component tree, hooks, tests, styles, and data boundary.
2. Describe the user interaction or external synchronization being changed.
3. Identify observable proof: accessible DOM, state transition, request, navigation, focus, or error behavior.
4. Run the narrowest useful baseline test when practical and record pre-existing failures.

Classify each step:

- `S` - rearranges components, hooks, state ownership, or props without changing observable behavior.
- `B` - changes rendering, interaction, accessibility, timing, network synchronization, or a public component contract.

## Choose when to tidy

State one decision:

- `before` - a small known cleanup immediately exposes the state transition or interaction to change.
- `after` - the behavior must be implemented to reveal the correct boundary.
- `later` - the cleanup is useful but too large or weakly related to the current behavior.
- `never` - no likely reuse or future change justifies it.

Do not tidy merely because a component is long. Tidy when a specific change is hard to understand, localize, or verify.

## Execute a reversible sequence

1. Propose the smallest `S -> UI checks -> B -> UI checks` sequence.
2. Apply one structural move at a time.
3. Preserve rendered and interaction behavior after each `S` step.
4. Add the requested behavior separately.
5. Stop when the behavior is proven. Record unrelated opportunities instead of expanding scope.

Do not create commits unless asked. If commits are requested, keep structural and behavioral changes separately reviewable.

## Select React moves

Read [references/guide.md](references/guide.md) before changing state ownership, Effects, or component boundaries. Prefer:

- pure rendering from props, state, and context;
- event handlers for interaction-caused work;
- Effects only for synchronization with external systems;
- derived values during render instead of redundant synchronized state;
- one authoritative owner for state that must stay synchronized;
- state close to the smallest subtree that needs it;
- a wrapper or adapter component for gradual prop/API migration;
- explicit props or hook inputs instead of hidden module state;
- component or hook extraction only around a coherent responsibility;
- temporary inlining when many tiny components obscure the interaction;
- comments for external constraints, not narration of JSX.

Do not introduce an Effect, memoization, context, global state, or a custom hook without a concrete requirement. Avoid snapshot-only evidence when an interaction assertion is available.

## Analyze coupling precisely

Name a change class:

`Changing <state/interaction/prop> requires coordinated edits in <components/hooks/tests/styles>.`

Use the actual component tree and co-change evidence. Co-locate code that repeatedly changes together; separate code that changes for different reasons.

## Verify and report

Use repository-native typecheck, lint, unit, component, and end-to-end checks in proportion to risk. Check accessible roles and interaction outcomes when relevant.

Report:

- user-visible behavior and proof;
- timing decision;
- structural moves and their purpose;
- behavioral change;
- checks run and results;
- remaining risk or one bounded deferred item.

