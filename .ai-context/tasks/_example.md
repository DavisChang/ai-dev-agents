# Task: Add Search Functionality to Content Picker

> This is an example task file showing how the three-phase workflow works with schema v2.0.
> Use `task_template.yaml` (YAML) or `task_template.md` (Markdown) as the starting point.

## Status

- [x] Phase 1: Inventory (盤點)
- [x] Phase 2: Proposal (提案)
- [ ] Phase 3: Implementation (實作)
- [ ] Review & Merge

## Requirements (需求描述)

**User Story / Description:**

As a teacher using the Content Picker, I want to search for content by title and description so that I can quickly find relevant teaching materials.

**Acceptance Criteria:**

- [x] Search bar appears at the top of ContentsPicker
- [x] Search filters content in real-time as user types (debounced 300ms)
- [x] Search works across content title and description fields
- [x] Empty search shows all content (default behavior)
- [x] Search state is preserved when navigating between tabs

**Priority:** High
**Estimated Scope:** Medium

---

## Phase 1: Inventory (盤點現況)

### Affected Features

| Feature ID | Feature Name | Impact Level | Notes |
|------------|-------------|-------------|-------|
| feat:contents-picker | ContentsPicker | high | Main feature — adding search UI and logic |
| feat:resources-picker | ResourcesPicker | medium | May want consistent search UX later |
| feat:view-lessons | ViewLessons | low | Search could be extended here in future |

### Affected Files

| File Path | Change Type | Description |
|-----------|------------|-------------|
| src/components/ContentsPicker/ContentsPicker.tsx | modify | Add SearchBar component |
| src/components/ContentsPicker/components/SearchBar.tsx | add | New search input component |
| src/components/ContentsPicker/hooks/useSearch.ts | add | New search hook with debounce |
| src/queries/content/getContentsDoc.ts | modify | Add search parameter to query |
| src/queries/content/useGetContents.ts | modify | Pass search param to query |

### Related Contracts

| Contract ID | Contract Name | Change Required | Breaking? |
|-------------|--------------|----------------|-----------|
| contract:graphql-content-api | Content GraphQL API | yes — add `search` param | no — additive |

### Cross-Repo Impact

| Repo | Impact | Coordination Needed |
|------|--------|-------------------|
| backend-api | Need search parameter support in GraphQL | yes — backend must deploy first |

### Existing Implementations to Reuse

- `useDebounce` hook already exists in `src/hooks/useDebounce.ts`
- Filter panel pattern in `src/components/ContentsPicker/components/FilterPanel.tsx`
- Existing query parameter handling in `useGetContents.ts`

### Constraints & Risks

| Risk | Severity | Mitigation |
|------|----------|-----------|
| Backend GraphQL API must support `search` first | HIGH | Mock with MSW during development |
| Search performance depends on backend | MEDIUM | Add loading state, backend adds index |
| i18n: placeholder text needs 40+ translations | LOW | Use English fallback |

---

## Phase 2: Proposal (技術方案)

### Architecture Decision

Add search as a client-side filter parameter that gets passed to the existing GraphQL `contents` query. Use the existing `useDebounce` hook to avoid excessive API calls. The SearchBar component will follow the existing FilterPanel pattern for consistency.

### Task Breakdown

#### Phase A: Search Hook & Component (Agent 04)

- [x] Step 1: Create `useSearch.ts` hook using existing `useDebounce`
- [x] Step 2: Create `SearchBar.tsx` component with i18n placeholder

#### Phase B: Integration (Agent 04)

- [x] Step 3: Modify `getContentsDoc.ts` to accept search variable
- [x] Step 4: Modify `useGetContents.ts` to pass search parameter
- [x] Step 5: Integrate SearchBar into ContentsPicker

### Test Plan (Agent 05)

| Test Type | What to Test | File |
|-----------|-------------|------|
| Unit | useSearch hook debounce behavior | hooks/__tests__/useSearch.test.ts |
| Unit | SearchBar renders and accepts input | components/__tests__/SearchBar.test.tsx |
| Integration | Search filters content list | __tests__/ContentsPicker.search.test.tsx |

### Contract Changes

```graphql
# Addition to existing contents query (non-breaking)
query GetContents($filters: ContentFilters!, $search: String) {
  contents(filters: $filters, search: $search) {
    # ... existing fields
  }
}
```

### Key Decisions

| Decision | Rationale | Alternatives Considered |
|----------|-----------|----------------------|
| Server-side search via GraphQL | Better performance for large datasets | Client-side filter (rejected: too slow for 10k+ items) |
| 300ms debounce | Balance responsiveness vs API load | 500ms (too slow), 100ms (too many calls) |
| Reuse useDebounce hook | DRY principle, already tested | New debounce (unnecessary) |

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Backend not ready | medium | high | Use MSW mock in development |
| Search performance | low | medium | Add loading state, backend indexing |
| i18n missing translations | low | low | Use English fallback |

### KB Updates Needed

| File | Change |
|------|--------|
| feature_map.yaml | Update feat:contents-picker with search components |
| contract_registry.yaml | Add search parameter to contract:graphql-content-api |

---

## Phase 3: Implementation (實作記錄)

### Progress

- [ ] Step 1: useSearch hook created (Agent 04)
- [ ] Step 2: SearchBar component created (Agent 04)
- [ ] Step 3: GraphQL query updated (Agent 04)
- [ ] Step 4: useGetContents updated (Agent 04)
- [ ] Step 5: Integration complete (Agent 04)
- [ ] Tests written and passing (Agent 05)
- [ ] Lint / typecheck passing
- [ ] Knowledge base updated
- [ ] Code review completed (Agent 06)

### Notes / Deviations from Proposal

_(To be filled during implementation)_
