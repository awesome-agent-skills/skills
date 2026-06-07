# Frontend Skills Design — react-conventions & ui-completeness

Date: 2026-06-07

Two new frontend skills for the awesome-agent-skills marketplace. They follow the
same **detect → decide → act → report** rhythm as `pattern-enforcer`, so the three
frontend skills form one coherent family.

---

## Skill 1: `react-conventions`

Prescriptive, build-time skill. Enforces correct React data/forms patterns —
respecting the codebase's existing choices, prescribing best-practice defaults
only in a vacuum.

### Philosophy
Respect existing approach; prescribe the best-practice default only when no
pattern exists. Never introduces a second fetching/form library when one is
already established.

### Concerns
- **Data fetching** (incl. mutations + optimistic updates) → React Query default
- **Forms** → React Hook Form + Zod default
- **Client/server boundaries** (`"use client"`) → only when Next.js App Router detected

### Trigger boundary vs pattern-enforcer
- pattern-enforcer owns *visual components* (dropdowns, modals, styling)
- react-conventions owns *data + forms behavior* (fetching, mutations, validation)
- Complementary, not overlapping.

### Phases
- **Phase 0 — Detect framework & stack.** Read `package.json`: React Query / SWR /
  TanStack? RHF / Formik? Zod / Yup? Next.js App Router (`next` + `app/`)? This
  scan decides which concerns are relevant and what "existing approach" means.
- **Phase 1 — Per concern, match vs prescribe** (independently per concern):
  - Existing approach found → match it, surface it.
  - No approach found → scaffold the prescribed default correctly.
  - Different prescribed lib installed but unused → use what's installed first.

### Correctness checklists (when scaffolding the default)
**Data fetching (React Query)**
- Stable, structured query keys (`['user', id]`)
- `isLoading` / `isError` / `error` handled at call site (hands to ui-completeness)
- Mutations use `useMutation` + `invalidateQueries` (or `setQueryData` optimistic)
- Optimistic updates include rollback (`onError` restores previous cache)
- No `useEffect` + `fetch` + `useState` triad when React Query is chosen

**Forms (RHF + Zod)**
- Zod schema is single source of truth; types via `z.infer`, not duplicated
- `zodResolver` wired in; no hand-rolled validation
- Accessible error wiring (`aria-invalid`, error linked to input)
- Submit handler typed from schema; disabled/loading during submission

**Client/server boundaries (Next.js only)**
- `"use client"` pushed to the leaf that needs interactivity, not whole page
- Data fetching stays in server components where possible
- Flag `"use client"` on a file with no hooks/handlers (unnecessary)

**Match-existing case:** skip the prescription but still apply universal correctness
checks (stable keys, accessible form errors, optimistic rollback).

### Output & guardrails
Report the decision + evidence:
> "Built the user fetch with React Query (no existing pattern — set default).
> Query key `['user', userId]`, mutation invalidates `['user']` with optimistic
> rollback. Forms: matched existing Formik in `SignupForm.tsx` — did not introduce RHF."

Never: introduce a 2nd fetching/form lib; duplicate Zod types by hand; put
`"use client"` above a non-interactive component; silently swap the codebase's
choice. Always state matched-existing vs new-default with file evidence.

---

## Skill 2: `ui-completeness`

Audit skill. Runs after building any UI component to verify nothing's missing.

### Trigger
After the agent finishes building/substantially editing a UI component, it runs
this sweep before declaring done. Also invokable as `/ui-completeness <component>`.

### Audit dimensions
**A. The four states** (for components rendering fetched/async/list data):
- **Loading** — skeleton/spinner, not a blank flash
- **Error** — real error UI with retry, not a swallowed catch
- **Empty** — explicit "no results", not a zero-length list rendering nothing
- **Success** — the happy path

Components with no async data are exempt from the four-states audit.

**B. Responsive check:**
- Fixed pixel widths that should be fluid
- Overflow risks (long text, wide tables, horizontal scroll)
- Desktop-only layout assumptions (no wrap, fixed columns)
- Touch targets too small on mobile

### Phases
- **Phase 0 — Classify component.** Renders async/list data? No → responsive check
  only. Yes → both dimensions.
- **Phase 1 — Audit against existing pattern.** Scan how the codebase already does
  loading/empty/error (`<Skeleton>`, `<Spinner>`, `<EmptyState>`, `<ErrorCard>`).
  Match those; invent only if none exists.
- **Phase 2 — Report gaps, then fix** using the matched pattern, concretely.
- **Phase 3 — Confirm the sweep:** `✓ loading ✓ error ✓ empty ✓ success ✓ responsive`

### Guardrails
Never: invent a state component when the codebase has one; flag four states on a
component with no async data; report without offering the inline fix. Always show
responsive changes as concrete before→after.

---

## Packaging
Two separate skills (not bundled), each with `.claude-plugin/plugin.json`,
`.codex-plugin/plugin.json`, `skills/<name>/SKILL.md`, and an examples session.
Add both to `.claude-plugin/marketplace.json`, `.agents/plugins/marketplace.json`,
and the README skills table.
