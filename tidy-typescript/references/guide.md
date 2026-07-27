# TypeScript empirical design guide

## Source model

This skill adapts Kent Beck's *Tidy First?* / Russian edition *Чистый дизайн. Практика эмпирического проектирования ПО* (2024). Use these ideas as decision heuristics, not universal style rules.

| Idea | Pages | Operational meaning |
|---|---:|---|
| Small behavior-preserving tidying moves | 31-64 | Change one comprehensible structural relation at a time. |
| Separate structure and behavior | 67-70 | Make the diff reviewable and make accidental behavior changes visible. |
| Chain cautiously and keep batches small | 71-87 | Stop speculative cleanup before conflict, interaction, and review costs dominate. |
| Untangle mixed work | 89-91 | Rebuild a coherent sequence when exploration mixed structure and behavior. |
| Tidy before, after, later, or never | 93-100 | Choose timing from immediate value, cost, reuse, confidence, and likely future change. |
| Structure creates future options | 111-115, 125-133 | Pay for flexibility only where uncertainty and future changes make it valuable. |
| Prefer reversible structural decisions | 135-138 | Make experiments cheap to undo. |
| Define coupling relative to a change | 139-143 | Name which change propagates between which elements. |
| Cohesion localizes co-changing elements | 157-159 | Group elements that change together and separate unrelated ones. |
| Stop after the useful change is enabled | 161-164 | Do not turn cleanup into the product goal. |

## TypeScript mappings

| Friction | Small move | Guardrail |
|---|---|---|
| Nested branches obscure the main path | Introduce a guard clause and let control-flow analysis narrow the remainder. | Preserve thrown/returned values and side-effect order. |
| A value enters as `any` or a broad bag | Accept `unknown` at the boundary, validate or narrow it, and pass an explicit typed value inward. | Do not replace runtime validation with a type assertion. |
| Callers struggle with an old signature | Add a typed adapter with the desired interface over the existing implementation. | Migrate callers incrementally; remove the old path only after evidence shows no consumers. |
| A discriminated union is handled inconsistently | Normalize handler shape and add an exhaustive `never` check where appropriate. | Do not add variants during a structural-only step. |
| Hidden environment or singleton access complicates tests | Read once at a boundary and pass a typed dependency/config object. | Keep the object cohesive; avoid a universal dependency bag. |
| A large expression is hard to change | Name domain subexpressions or constants. | Names must encode meaning, not syntax. |
| Helpers obscure flow | Inline the relevant path temporarily, observe structure, then extract cohesive helpers. | Preserve evaluation order and exception behavior. |
| Suspected dead export | Check references, dynamic registration, package exports, tests, and runtime evidence as needed. | TypeScript reference search alone may miss reflection, string lookup, or external consumers. |

## Type-level safety

- Use `strict` when the repository already supports it; do not broaden the task into a compiler migration.
- Prefer narrowing over assertions.
- Prefer `unknown` to `any` at untrusted boundaries.
- Use the fewest generic parameters needed to relate inputs and outputs.
- Prefer a union parameter over overloads when behavior and return type are the same.
- Preserve the distinction between an absent optional property and a present `undefined` value when repository settings or runtime semantics care about it.
- Treat typecheck as necessary but insufficient evidence of runtime behavior.

## Current primary references

- TypeScript narrowing: https://www.typescriptlang.org/docs/handbook/2/narrowing.html
- TypeScript functions and generic design: https://www.typescriptlang.org/docs/handbook/2/functions.html
- `strict`: https://www.typescriptlang.org/tsconfig/strict.html
- `exactOptionalPropertyTypes`: https://www.typescriptlang.org/tsconfig/exactOptionalPropertyTypes.html
- `noUncheckedIndexedAccess`: https://www.typescriptlang.org/tsconfig/noUncheckedIndexedAccess.html

