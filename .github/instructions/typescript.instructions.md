---
description: "Use when writing or editing TypeScript or TSX files. Covers patterns specific to this codebase: React Ink, Bun bundler, Zod schemas, tool/command naming, feature flags."
applyTo: "**/*.{ts,tsx}"
---
# TypeScript Conventions (this codebase)

## Naming
- Tools: TitleCase + "Tool" suffix (e.g. `BashTool`, `FileEditTool`)
- Commands: lowercase, `/` prefix for slash commands
- Constants: UPPER_SNAKE_CASE, exported from `prompt.ts` or `constants.ts` per tool

## Patterns
- Zod schemas for validation, lazy-loaded to avoid circular dependencies
- Feature flags via `feature()` from `bun:bundle` — these are compile-time, not runtime
- System prompts are `string[]` arrays, not single strings
- Use `appendSystemPrompt` for additions, never mutate the array directly
- Lazy `require()` behind `feature()` guards for dead-code elimination

## File Layout
- Tool implementation: `tools/<ToolName>/<ToolName>.tsx`
- Tool constants/prompt: `tools/<ToolName>/constants.ts` or `prompt.ts`
- Commands: `commands/<name>/<name>.tsx`
- Services: `services/<ServiceName>/`
- State selectors: `state/selectors/`

## Anti-patterns to Avoid
- Do not import feature-gated modules at top level — use conditional `require()` inside `feature()` checks
- Do not use `as any` to silence type errors — fix the type
- Do not add `@ts-ignore` without an explanatory comment
- Do not create barrel `index.ts` files unless the folder already has one
