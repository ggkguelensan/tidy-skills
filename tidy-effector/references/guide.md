# Effector empirical design guide

## Source model

This skill adapts Kent Beck's *Tidy First?* / Russian edition *Чистый дизайн. Практика эмпирического проектирования ПО* (2024).

| Idea | Pages | Effector interpretation |
|---|---:|---|
| Small tidying moves | 31-64 | Rewire or rename one unit relation while preserving graph behavior. |
| Separate structure and behavior | 67-70 | Keep graph reshaping distinguishable from state-transition changes. |
| Chains and small batches | 71-87 | Avoid cascading unit migrations that outgrow current evidence. |
| Tidy before, after, later, or never | 93-100 | Choose timing from the requested event-to-result path. |
| Structure creates options | 111-115 | Pay for model flexibility where change uncertainty is real. |
| Reversibility | 135-138 | Prefer compatibility events, adapters, and isolated Scope tests. |
| Coupling relative to change | 139-143 | Name the payload, state shape, or effect contract that propagates. |
| Cohesion | 157-159 | Group the units that implement one business transition. |

## Unit roles

- Event: a fact, intent, or entry point that carries a payload.
- Store: current application state derived from events and other units.
- Effect: work that touches an external system or has success/failure semantics.
- `sample`: a declarative edge that reads source state when a clock occurs, optionally filters/transforms, and sends to a target.
- Scope: an isolated instance of the application graph for SSR, testing, and concurrent execution.

## Effector mappings

| Friction | Small move | Guardrail |
|---|---|---|
| Business logic reads `$store.getState()` | Put the store in `sample.source` and trigger from an explicit clock. | Preserve the exact sampling moment and concurrency semantics. |
| `watch` triggers business work | Replace it with `sample` targeting an event or effect. | Keep `watch` for debugging or integration boundaries only. |
| Async or fallible work occurs in a pure transform | Move it to an effect and connect via `sample`. | Keep `sample.fn` and filters pure. |
| React calls imported units directly | Bind/read units with `useUnit`. | Ensure the component is under the intended Provider/Scope when required. |
| External callback loses Scope | Bind the re-entry unit with `scopeBind` inside the correct scope. | Check timers, sockets, listeners, and third-party promises. |
| A model API is awkward for a new caller | Add a compatibility event or adapter that maps to the old graph. | Migrate consumers before deleting the old entry point. |
| The same unit structure repeats | Extract a factory with explicit dependencies and returned public units. | Do not create a factory from one speculative instance. |
| One flow is scattered across many files | Co-locate its event, state, effect, and wiring, or temporarily inline the graph to see the flow. | Preserve static initialization and avoid runtime unit creation without a requirement. |
| Store objects are mutated | Return a new reference from updates. | Preserve equality and update-skipping behavior. |

## Scope-safe verification

1. Create a fresh `fork()` for each scenario.
2. Supply initial `values` and mocked effect `handlers` through the Scope.
3. Trigger the public entry unit with `allSettled`.
4. Assert final store state and important effect/event outcomes.
5. Add a failure or concurrency scenario when the behavior involves async work.
6. Check SSR serialization or client Scope binding when the application uses them.

## Current primary references

- Best practices: https://effector.dev/en/guides/best-practices
- Events: https://effector.dev/en/essentials/events
- State management: https://effector.dev/en/essentials/manage-states
- `sample`: https://effector.dev/en/api/effector/sample
- `useUnit`: https://effector.dev/en/api/effector-react/useUnit
- Troubleshooting and Scope loss: https://effector.dev/en/guides/troubleshooting
- Testing: https://effector.dev/en/guides/testing

