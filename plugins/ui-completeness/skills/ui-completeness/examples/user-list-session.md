# Example: Auditing a List Component After Building It

## Context
The agent just built `UserList` that fetches and renders users. Before declaring
it done, ui-completeness runs.

## Phase 0 — Classify
```
grep -nE "useQuery|.map\(|isLoading" UserList.tsx
→ useQuery present, users.map() present  → async/list component → both dimensions
```

## Phase 1 — Audit against existing pattern
```
grep -rl "Skeleton\|EmptyState\|ErrorCard" --include="*.tsx" src
→ src/components/ProductList.tsx uses <EmptyState>
→ src/components/ui/ErrorCard.tsx exists
→ src/components/ui/Skeleton.tsx exists
```

## Phase 2 — Report gaps, then fix

> "`UserList` handles **success** and **loading**, but:
> - **No empty state** — renders nothing when `users` is `[]`
> - **Error swallowed** — caught but only `console.log`'d
> - **Responsive** — fixed `w-[600px]` will overflow on mobile
>
> Fixing using existing components: `<EmptyState>` (matches `ProductList`), `<ErrorCard>` with retry, and `w-[600px]` → `w-full max-w-[600px]`."

### Before
```tsx
function UserList() {
  const { data: users, isLoading } = useUsers()
  if (isLoading) return <Skeleton />
  return (
    <div className="w-[600px]">
      {users.map((u) => <UserRow key={u.id} user={u} />)}
    </div>
  )
}
```

### After
```tsx
function UserList() {
  const { data: users, isLoading, isError, refetch } = useUsers()

  if (isLoading) return <Skeleton />
  if (isError) return <ErrorCard onRetry={refetch} />        // matched existing pattern
  if (!users?.length) return <EmptyState message="No users yet" />  // matched ProductList

  return (
    <div className="w-full max-w-[600px]">                   // fluid, won't overflow
      {users.map((u) => <UserRow key={u.id} user={u} />)}
    </div>
  )
}
```

## Phase 3 — Confirm the sweep
> ✓ loading ✓ error ✓ empty ✓ success ✓ responsive

---

# Example: Static Component — Four States Skipped

## Context
The agent built a `PageHeader` with a title and a static action button.

## Phase 0 — Classify
```
grep -nE "useQuery|.map\(|isLoading" PageHeader.tsx → no matches
→ static/presentational → responsive check only, four-states skipped
```

## Phase 2 — Responsive only
> "`PageHeader` is static — no data states apply. Responsive: title uses `text-3xl` with no mobile size; long titles won't wrap. Fixing: added `truncate` and `text-xl sm:text-3xl`."

## Phase 3 — Confirm
> ✓ responsive (static component — data states not applicable)
