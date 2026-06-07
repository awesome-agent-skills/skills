---
name: ui-completeness
description: Use after building or substantially editing any React UI component, before declaring it done. Sweeps the component for the four data states (loading, error, empty, success) and for responsive issues (fixed widths, overflow, desktop-only layout, small touch targets), then fixes gaps using the codebase's existing state components. Skips the four-states check on components with no async data. Triggered automatically on finishing a UI component, or by "/ui-completeness <component>".
---

# UI Completeness

## Overview

A verification pass that runs after a UI component is built. It catches the two most common omissions — missing data states and broken responsiveness — and fixes them using whatever patterns the codebase already has. One rule: a component is not done until every state it can be in actually renders.

This skill verifies what `react-conventions` set up: react-conventions wires `isLoading` / `isError` at the data layer; ui-completeness confirms those surface as real *UI*.

## Invocation

```
/ui-completeness <component>
```

Also runs automatically when finishing or substantially editing a UI component.

## Phase 0: Classify the Component

Does the component render async, fetched, or list data?

- **No** (static / presentational — a button, a layout, a label) → run the **responsive check only**. Skip the four-states audit entirely; do not nag about loading/empty states that cannot apply.
- **Yes** → run **both** dimensions below.

Detect async/list data:
```bash
grep -nE "useQuery|useSWR|useMutation|\.map\(|isLoading|data\?" <component-file>
```

## Phase 1: Audit Against the Existing Pattern

Before suggesting any state UI, scan how this codebase already renders these states, and match it. Only invent one if none exists.

```bash
grep -rl "Skeleton\|Spinner\|Loading\|EmptyState\|NoResults\|ErrorCard\|ErrorState" --include="*.tsx" src | head
```

Note which components are used for: loading, empty, and error. These are what you will reuse.

## Phase 2: Report Gaps, Then Fix

### Dimension A — The four states (async/list components)
- **Loading** — a skeleton or spinner, not a blank flash
- **Error** — a real error UI with retry, not a swallowed `catch` or a bare `console.log`
- **Empty** — an explicit "no results" state, not a zero-length list that renders nothing
- **Success** — the happy path (usually the only one already built)

### Dimension B — Responsive
- Fixed pixel widths that should be fluid (`w-[600px]` on a container)
- Overflow risks — long text, wide tables, horizontal scroll
- Desktop-only layout — no wrapping, fixed column counts
- Touch targets too small on mobile

Report the gaps concretely, then implement the fix using the matched pattern:

> "`UserList` handles success + loading, but: **no empty state** (renders nothing when `users` is `[]`), and **error is swallowed** (caught, only logged). Responsive: fixed `w-[600px]` will overflow on mobile.
> Fixing: added `<EmptyState>` (matches `ProductList`), surfaced the error via the existing `<ErrorCard>` with retry, changed `w-[600px]` → `w-full max-w-[600px]`."

## Phase 3: Confirm the Sweep

End with a one-line coverage checklist so the user sees what was verified:

> ✓ loading ✓ error ✓ empty ✓ success ✓ responsive

For a static component, only the relevant marks apply:

> ✓ responsive (static component — data states not applicable)

## Rules

- Never invent a new state component when the codebase already has one — match it
- Never flag the four states on a component with no async data
- Never just report — always offer the fix inline; auditing without fixing is half the value
- Always show responsive changes as a concrete before → after
- Always end with the coverage checklist
