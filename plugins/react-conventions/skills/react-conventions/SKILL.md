---
name: react-conventions
description: Use before building React data fetching, mutations, or forms. Detects the codebase's existing approach and matches it; prescribes a best-practice default (React Query for fetching, React Hook Form + Zod for forms) only when no pattern exists. Adds Next.js client/server boundary guidance only when Next.js App Router is detected. Triggered by "add a fetch", "build a form", "call this API", "mutation", or "/react-conventions <what to build>".
---

# React Conventions

## Overview

Build React data and form code the right way — without fighting the codebase. One rule: match the approach that already exists; only when there is none, scaffold the best-practice default correctly.

This skill owns **data and forms behavior** (fetching, mutations, validation). It is complementary to `pattern-enforcer`, which owns **visual components** (dropdowns, modals, styling). When both could apply, pattern-enforcer handles the component shell; react-conventions handles the data and form logic inside it.

## Invocation

```
/react-conventions <what you want to build>
```

Also activates automatically when about to build a data fetch, a mutation, or a form.

## Phase 0: Detect Framework & Stack

Read `package.json` once. Determine what is present:

| Concern | Look for | Prescribed default (vacuum only) |
|---------|----------|----------------------------------|
| Data fetching | `@tanstack/react-query`, `react-query`, `swr` | React Query |
| Forms | `react-hook-form`, `formik` | React Hook Form |
| Schema validation | `zod`, `yup` | Zod |
| Framework | `next` + an `app/` directory | (enables Next.js boundary checks) |

```bash
cat package.json | grep -E "react-query|@tanstack|swr|react-hook-form|formik|zod|yup|\"next\""
ls -d app 2>/dev/null || ls -d src/app 2>/dev/null
```

The Next.js client/server boundary checks in Phase 2 activate **only** if Next.js App Router is detected. In a Vite/CRA/Remix SPA they are skipped silently.

## Phase 1: Decide Match vs Prescribe (per concern)

Run this decision independently for each concern in the request. A project may have an established fetching pattern but no form pattern yet.

- **Existing approach found** (e.g. SWR used in 5+ places): match it. Surface it.
  > "Codebase uses SWR — following that, not introducing React Query."
- **A prescribed lib is installed but unused** (e.g. RHF in `package.json`, no forms yet): use what's installed before adding anything new.
- **No approach found** (greenfield / first fetch / first form): scaffold the prescribed default, applying the correctness checklist in Phase 2.

To detect an existing approach, count real usages:

```bash
# fetching
grep -rl "useQuery\|useMutation\|useSWR\|useEffect.*fetch" --include="*.ts*" src | head
# forms
grep -rl "useForm\|<Formik\|register(\|handleSubmit" --include="*.ts*" src | head
```

## Phase 2: Apply the Correctness Checklist

### Data fetching (React Query)
- Stable, structured query keys — `['user', id]`, never string concatenation
- `isLoading` / `isError` / `error` handled at the call site (the actual UI states are owned by `ui-completeness`)
- Mutations use `useMutation` + `invalidateQueries` (or `setQueryData` for optimistic), never a manual refetch
- Optimistic updates include the rollback path — `onError` restores the previous cache
- Do **not** write the `useEffect` + `fetch` + `useState` triad when React Query is the chosen tool

### Forms (React Hook Form + Zod)
- The Zod schema is the single source of truth; derive types with `z.infer`, do not duplicate them
- `zodResolver` wired in; no hand-rolled validation
- Each field has accessible error wiring — `aria-invalid` and the error message linked to the input
- The submit handler is typed from the schema; disable/show-loading during submission

### Client/server boundaries (Next.js App Router only)
- Push `"use client"` to the **leaf** that needs interactivity, not the whole page
- Keep data fetching in server components where possible; React Query lives under a client boundary
- Flag `"use client"` at the top of a file that has no hooks or handlers — it is unnecessary

**Match-existing case:** skip the library prescription, but still apply the *universal* correctness checks above (stable query keys, accessible form errors, optimistic rollback). These are correctness, not library preference.

## Phase 3: Report

State the decision and the evidence so the choice is auditable:

> "Built the user fetch with **React Query** (no existing fetching pattern found — set the default). Query key `['user', userId]`, mutation invalidates `['user']` on success with optimistic update + rollback. Forms: matched existing **Formik** in `SignupForm.tsx` — did not introduce RHF."

## Rules

- Never introduce a second fetching or form library when one is already established
- Never duplicate a Zod schema's types by hand
- Never put `"use client"` above a component that has no interactivity
- Never silently swap the codebase's choice for a "better" one — match it, or ask if truly ambiguous
- Always state which path was taken (matched existing vs. set new default) and the file evidence
