---
name: pattern-enforcer
description: Use before implementing any UI component, error handling, test, or named symbol. Scans the codebase for existing patterns and enforces consistency — never introduces a new approach when one already exists. Triggered by "add a dropdown", "write a test for", "handle this error", "create a new file/function/class", or "/pattern-enforcer <what to implement>".
---

# Pattern Enforcer

## Overview

Scan before you act. Find how it's already done in this codebase, then do it the same way. One rule: never introduce a new pattern when an existing one can be reused.

## Invocation

```
/pattern-enforcer <what you want to implement>
```

Also activates automatically when Claude is about to implement anything in the four domains: UI components, error handling, tests, or named symbols.

## Phase 0: Detect Domain

Classify the request into one or more domains:

| Request mentions | Domain |
|-----------------|--------|
| button, dropdown, modal, form, input, select, checkbox, table, card, toast, dialog | **UI** |
| try/catch, error, exception, 404, 500, validation, raise, throw | **Error handling** |
| test, spec, describe, it, expect, assert, mock, fixture | **Tests** |
| new file, new function, new class, new variable, new hook, new service, new controller | **Naming** |

A single request can trigger multiple domains. Run each that applies.

## Phase 1: Scan

### UI Domain
Search for existing usage of the same element type:
```bash
grep -r "dropdown\|<Select\|<Combobox\|<Listbox" --include="*.tsx" --include="*.jsx" --include="*.vue" -l
grep -r "modal\|<Dialog\|<Modal" --include="*.tsx" --include="*.jsx" -l
# adjust keywords to match the element being added
```
Read 2–3 of the found files to understand:
- Which component/library is used (shadcn, MUI, custom, etc.)
- How it's imported
- What props are passed
- How it's styled (Tailwind classes, CSS modules, inline)

### Error Handling Domain
```bash
grep -r "catch\|rescue\|except\|AppError\|ApiError\|raise\|throw new" --include="*.ts" --include="*.rb" --include="*.py" -l | head -10
```
Read 2–3 found files to understand:
- Error class hierarchy (custom vs native)
- How errors are logged
- What user-facing error messages look like
- HTTP status codes used

### Tests Domain
```bash
# Find tests for the same layer as what's being tested
find . -name "*.test.*" -o -name "*.spec.*" -o -name "*_test.*" | head -20
```
Read the closest existing test file (same directory or same module) to understand:
- Test framework and imports
- `describe`/`it`/`test` structure
- How mocks are set up
- Assertion style (`expect`, `assert`, `should`)
- Helper functions used

### Naming Domain
```bash
# Sample file names in the same directory
ls -la <target-directory>

# Sample function/class names in similar files
grep -r "export function\|export class\|export const\|def \|class " <adjacent-files> | head -20
```
Extract: casing convention (camelCase, snake_case, PascalCase, kebab-case), prefix/suffix patterns (e.g. `useX` for hooks, `XService` for services, `XController`).

## Phase 2: Assess Findings

**One clear pattern found:**
> "Found 8 dropdowns using `<Select>` from shadcn/ui, imported from `@/components/ui/select`, styled with Tailwind. Implementing the same way."

Proceed to Phase 3 immediately.

**Multiple patterns found:**
> "Found two approaches:
> 1. `<Select>` from shadcn/ui — used in 6 places (settings, profile, filters)
> 2. Native `<select>` — used in 2 older places (legacy forms)
>
> Which should I use for this?"

Wait for user choice, then proceed to Phase 3.

**No pattern found:**
> "No existing [element type] found in this codebase. This will be the first one.
> Based on your stack ([detected stack]), the standard approach is [best-practice recommendation].
> Proceeding with that — this will set the pattern for future use."

Proceed to Phase 3 using the best-practice approach for the detected stack.

## Phase 3: Implement

Implement using exactly the pattern identified:
- Same component / same import path
- Same props structure
- Same styling approach
- Same error class hierarchy
- Same test structure and helpers
- Same naming convention

After implementing, add a brief note:
> "Used `<Select>` from shadcn/ui — consistent with [filename] and 7 other places."

## Rules

- Never import a new UI library if one already exists in the project
- Never create a new error class if an existing one covers the case
- Never write a test in a different style from adjacent tests
- Never name a file/function using a different casing from its neighbors
- If two patterns exist and the user doesn't specify, always pick the more widely used one (higher count wins)
- Do not refactor existing inconsistencies unless the user explicitly asks — just match the dominant pattern
- Always show which files you found the pattern in so the user can verify
