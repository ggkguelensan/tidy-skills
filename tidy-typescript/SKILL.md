---
name: tidy-typescript
description: Plan, implement, and review small behavior-preserving TypeScript structure changes using Kent Beck's empirical software-design method. Use for .ts or .tsx feature and bug work in hard-to-change code, focused refactoring or cleanup, typed API migration, change-coupling analysis, or review of diffs that mix structure and behavior.
---

# Tidy TypeScript

Make the requested behavior change easier through the smallest justified structural steps. Treat cleanup as a local experiment, not as permission to redesign the codebase.

## Establish the change

1. Read repository instructions and inspect the affected call path, tests, configuration, and package scripts.
2. Restate the requested observable behavior and the evidence that will prove it.
3. Run the narrowest useful baseline checks when practical. Record pre-existing failures.
4. Identify the exact friction blocking the change. Do not tidy unrelated code.

Classify each proposed step:

- `S` - changes structure while preserving runtime behavior and public contracts.
- `B` - changes runtime behavior or a consumer-visible type/API contract.

Treat a public TypeScript type change as `B` when it changes what callers may compile or must provide.

## Choose when to tidy

Choose one and state the reason:

- `before` - a known, small structural step immediately clarifies or reduces the requested change.
- `after` - implementing the behavior reveals the useful structure, and delaying cleanup would lose context.
- `later` - cleanup is larger than the current change but has credible future value; record a bounded follow-up.
- `never` - the code is unlikely to change again, cleanup provides no information, or its cost exceeds its value.

Prefer `before` only when the expected relation is concrete:

`cost(S) + cost(B after S) < cost(B without S)`

## Execute a reversible sequence

1. Propose the shortest `S -> checks -> B -> checks` sequence.
2. Apply one cognitively small structural move at a time.
3. Re-run the cheapest check able to catch accidental behavior change after each risky step.
4. Implement the requested behavior as a distinct logical change.
5. Stop when the behavior is enabled and verified. Defer attractive but unnecessary cleanup.

Do not create commits or rewrite history unless the user asks. When commits are in scope, keep structural and behavioral changes separately reviewable.

## Select TypeScript moves

Read [references/guide.md](references/guide.md) when selecting a move or reviewing a TypeScript diff. Prefer:

- guard clauses that improve control-flow narrowing;
- removal of dead code only with adequate static, test, or runtime evidence;
- one consistent representation for the same operation;
- a typed adapter exposing a better interface over the old implementation;
- reading order from public operation to implementation detail;
- co-location of elements that change together;
- declaration and initialization near first meaningful use;
- explanatory names for domain expressions and literals;
- explicit typed inputs instead of hidden globals, environment reads, or broad option bags;
- extraction of a helper only when the block has a coherent purpose and limited interaction;
- temporary inlining when fragmentation hides the real data flow;
- comments for constraints and reasons that code cannot express.

Never use `any`, unchecked assertions, non-null assertions, or disabled compiler rules merely to make a structural change pass.

## Analyze coupling precisely

Name both elements and the class of change:

`Changing <element A> with respect to <change> also requires changing <element B>.`

Use call sites, imports, type consumers, tests, and co-change history as evidence. Do not call two modules "coupled" solely because one imports the other.

## Verify and report

Use repository-native formatting, lint, typecheck, and tests. Prefer targeted checks first, then broader checks in proportion to risk.

Report:

- requested behavior and proof;
- timing decision: `before`, `after`, `later`, or `never`;
- structural moves made and why each was necessary;
- behavioral change;
- checks run and their results;
- remaining risk or one bounded deferred item.

