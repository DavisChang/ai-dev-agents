# Task: [Task Title]

> **For AI agents**: Use the YAML version (`task_template.yaml`) for structured parsing.
> **For humans**: Use this Markdown version for readability. Copy to `.ai-context/tasks/your-task-name.md`.
>
> Follow the three-phase workflow: Inventory -> Proposal -> Implementation.

## Status

- [ ] Phase 1: Inventory (盤點)
- [ ] Phase 2: Proposal (提案)
- [ ] Phase 3: Implementation (實作)
- [ ] Review & Merge

## Requirements (需求描述)

<!-- Fill in: What needs to be done? What is the acceptance criteria? -->

**User Story / Description:**


**Acceptance Criteria:**

- [ ] Criteria 1
- [ ] Criteria 2
- [ ] Criteria 3

**Priority:** High / Medium / Low
**Estimated Scope:** Small / Medium / Large

---

## Phase 1: Inventory (盤點現況)

<!-- AI fills this section after analyzing the codebase -->

### Affected Features

<!-- Which features from feature_map.yaml are impacted? -->

| Feature ID | Feature Name | Impact Level | Notes |
|------------|-------------|-------------|-------|
| | | high / medium / low | |

### Affected Files

<!-- Key files that will need changes -->

| File Path | Change Type | Description |
|-----------|------------|-------------|
| | add / modify / delete | |

### Related Contracts

<!-- API contracts from contract_registry.yaml that are affected -->

| Contract ID | Contract Name | Change Required | Breaking? |
|-------------|--------------|----------------|-----------|
| | | yes / no | yes / no |

### Cross-Repo Impact

<!-- From repo_manifest.yaml — other repos affected -->

| Repo | Impact | Coordination Needed |
|------|--------|-------------------|
| | | yes / no |

### Existing Implementations to Reuse

<!-- Code that already exists and can be leveraged -->


### Constraints & Risks

<!-- From environment_map.yaml, coding_standards.yaml, etc. -->


---

## Phase 2: Proposal (技術方案)

<!-- AI generates this after Inventory is complete and reviewed -->

### Architecture Decision

<!-- High-level approach and rationale -->


### Task Breakdown

<!-- Ordered list of implementation steps -->

#### Phase A: [Name]

- [ ] Step 1:
- [ ] Step 2:

#### Phase B: [Name]

- [ ] Step 1:
- [ ] Step 2:

### Contract Changes

<!-- If any API contracts need updating -->


### Test Plan

<!-- What tests will be written -->

| Test Type | What to Test | File |
|-----------|-------------|------|
| Unit | | |
| Integration | | |

### Key Decisions

<!-- Trade-offs made and their rationale -->

| Decision | Rationale | Alternatives Considered |
|----------|-----------|----------------------|
| | | |

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| | low/med/high | low/med/high | |

---

## Phase 3: Implementation (實作記錄)

<!-- Updated as implementation progresses -->

### Progress

- [ ] Task 1: description
- [ ] Task 2: description
- [ ] Tests written and passing
- [ ] Lint / typecheck passing
- [ ] Knowledge base updated (feature_map, contracts)
- [ ] Code review completed

### Changes Made

<!-- Summary of actual changes for reference -->

| File | Change | Lines |
|------|--------|-------|
| | | |

### Notes / Deviations from Proposal

<!-- Document any deviations from the approved proposal -->

