# React empirical design guide

## Source model

This skill adapts Kent Beck's *Tidy First?* / Russian edition *Чистый дизайн. Практика эмпирического проектирования ПО* (2024).

| Idea | Pages | React interpretation |
|---|---:|---|
| Small tidying moves | 31-64 | Move one state, prop, expression, or component boundary without changing UI behavior. |
| Separate structure and behavior | 67-70 | Keep component reshaping distinguishable from interaction changes. |
| Small batches and short rhythm | 77-87 | Avoid broad component-tree rewrites before one feature. |
| Tidy before, after, later, or never | 93-100 | Choose timing based on the next user-visible change. |
| Structure creates options | 111-115 | Value a component boundary when it makes credible future variants cheaper. |
| Reversibility | 135-138 | Prefer wrappers, adapters, and small moves that can be undone. |
| Coupling relative to change | 139-143 | Ask which UI or state change forces coordinated edits. |
| Cohesion | 157-159 | Keep state transitions and UI that change together close. |

## React mappings

| Friction | Small move | Guardrail |
|---|---|---|
| Nested rendering hides the main state | Use early returns for loading, error, empty, or permission states. | Preserve focus, announcements, and hook ordering. Never call hooks conditionally. |
| Derived data is synchronized with an Effect | Calculate it during render; memoize only if measurement or semantics justify it. | Preserve expensive computation and referential requirements deliberately. |
| Interaction logic lives in an Effect | Move it to the event handler that caused it. | Keep true external-system synchronization in an Effect. |
| Two state values can contradict each other | Replace them with one authoritative representation. | Define migration and reset behavior explicitly. |
| Sibling state must stay synchronized | Lift it to the closest common owner. | Do not lift unrelated transient state. |
| State is global but used by one subtree | Move it closer to that subtree. | Preserve navigation, persistence, and reset semantics. |
| Prop API makes the next change awkward | Add a wrapper or adapter with the desired props over the old component. | Treat consumer-visible prop changes as behavior. |
| JSX and logic are fragmented across tiny helpers | Inline the relevant interaction path, observe its shape, then extract a cohesive component or hook. | Do not extract merely to reduce line count. |
| Comment narrates markup | Rename or restructure until the comment is redundant. | Retain comments explaining browser, accessibility, or product constraints. |

## React invariants

- Keep rendering pure: do not mutate pre-existing props, state, context, or module data during render.
- Treat props, state, and context as read-only inputs.
- Put user-caused side effects in event handlers.
- Use Effects for synchronization with external systems, with correct dependencies and cleanup.
- Avoid redundant and duplicate state.
- Use keys intentionally when state reset is required; do not change keys accidentally during structural cleanup.
- Preserve accessible names, roles, keyboard behavior, focus, and error announcements.
- Prefer behavior assertions over large snapshots.

## Current primary references

- Keeping components pure: https://react.dev/learn/keeping-components-pure
- Choosing state structure: https://react.dev/learn/choosing-the-state-structure
- Sharing state: https://react.dev/learn/sharing-state-between-components
- Preserving and resetting state: https://react.dev/learn/preserving-and-resetting-state
- You might not need an Effect: https://react.dev/learn/you-might-not-need-an-effect
- Separating events from Effects: https://react.dev/learn/separating-events-from-effects

