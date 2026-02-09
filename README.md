# AI Dev Agents

**Give AI real project context to autonomously complete development tasks.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Cursor Compatible](https://img.shields.io/badge/Cursor-Compatible-blue.svg)](https://cursor.sh)
[![Schema Version](https://img.shields.io/badge/Schema-v2.0-green.svg)](./.ai-context/_meta.yaml)

---

## Table of Contents

- [Why](#why)
- [Architecture](#architecture)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Knowledge Base](#knowledge-base)
- [Agent Reference](#agent-reference)
- [Workflow](#workflow)
- [Optimization](#optimization)
- [Supported Stacks](#supported-stacks)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

---

## Why

### The Problem

AI coding tools (like Copilot, Cursor) without project context produce:

- **Hallucinated Code**: Calling non-existent functions, assuming non-existent APIs
- **Inconsistent Style**: Ignoring team conventions, different output each time
- **Reinventing the Wheel**: Not knowing existing features, re-implementing them
- **Breaking Contracts**: Modifying APIs without updating consumers

### The Solution

This project provides a **portable multi-agent system**, including:

1. **Structured Knowledge Base** (`.ai-context/`): Let AI understand your entire project
2. **8 Specialized Agents** (`.cursor/rules/`): Each with specific responsibilities, collaborating on development
3. **Metadata Tracking** (`_meta.yaml`): Track knowledge base health and completeness

After installation in any project, AI will automatically detect project type, scan codebase, build knowledge base, then follow the **Inventory -> Proposal -> Implementation** workflow for autonomous development.

---

## Architecture

### Three-Layer System

```
┌─────────────────────────────────────────────────────────┐
│  Layer 3: Agent Rules (.cursor/rules/)                  │
│                                                         │
│  00 Context Router  (always on, smart loading)          │
│  01 Bootstrap       (detect project type, build KB)     │
│  02 Inventory       (audit, structured output)          │
│  03 Proposal        (design, structured output)         │
│  04 Implementer     (code only, delegates tests)        │
│  05 Test Writer     (sole test owner)                   │
│  06 Reviewer        (quality gate, severity levels)     │
│  07 Meta Optimizer  (audit, drift detection)            │
│                                                         │
├─────────────────────────────────────────────────────────┤
│  Layer 2: Metadata (.ai-context/_meta.yaml)             │
│                                                         │
│  project_type       (drives conditional file loading)   │
│  completeness       (per-file scores 0.0 - 1.0)        │
│  gaps_by_owner      (PM / Designer / Backend / QA)      │
│  skipped_files      (N/A for this project type)         │
│  detection_failures (bootstrap errors)                  │
│                                                         │
├─────────────────────────────────────────────────────────┤
│  Layer 1: Knowledge Base (.ai-context/)                 │
│                                                         │
│  project_profile.yaml    (tech fingerprint)             │
│  feature_map.yaml        (features -> code, cross-refs) │
│  contract_registry.yaml  (API contracts, versioning)    │
│  repo_manifest.yaml      (ecosystem, deploy coord.)     │
│  environment_map.yaml    (envs, DB, cache, secrets)     │
│  coding_standards.yaml   (conventions, YAML format)     │
│  domain_glossary.yaml    (terms, roles, permissions)    │
│  design_system.yaml      (tokens, a11y - FE only)      │
│  data_models.yaml        (schema, indexes, queries)     │
│  test_strategy.yaml      (levels, mocking, flaky)       │
│  task_template.yaml      (structured task format)       │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Agent Workflow State Machine

```
                    ┌─────────────┐
                    │  Bootstrap  │  Agent 01: Detect project type,
                    │  (first run)│  scan codebase, populate KB
                    └──────┬──────┘
                           │ KB populated
                    ┌──────▼──────┐
                    │    Ready    │  Create task file in
                    │             │  .ai-context/tasks/
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  Agent 02   │  Phase 1: Inventory
                    │  Inventory  │  Structured output with
                    │             │  cross-reference IDs
                    └──────┬──────┘
                           │ User confirms
                    ┌──────▼──────┐
                    │  Agent 03   │  Phase 2: Proposal
                    │  Proposal   │  Architecture, task breakdown,
                    │             │  test plan, risks
                    └──────┬──────┘
                           │ User approves
                    ┌──────▼──────┐
                    │  Agent 04   │  Phase 3: Implementation
                    │ Implementer │  Code ONLY (no tests)
                    └──────┬──────┘
                           │ Per step or at end
                    ┌──────▼──────┐
                    │  Agent 05   │  Test Generation
                    │ Test Writer │  SOLE test owner
                    └──────┬──────┘
                           │ All steps done
                    ┌──────▼──────┐
                    │  Agent 06   │  Quality Gate
                    │  Reviewer   │  CRITICAL / WARNING / SUGGESTION
                    └──────┬──────┘
                           │ All CRITICALs fixed
                    ┌──────▼──────┐
                    │   Merge     │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  Agent 07   │  Update _meta.yaml,
                    │  Optimizer  │  detect drift, suggest improvements
                    └─────────────┘
```

### Agent Summary

| Agent | Name | Trigger | Role |
|-------|------|---------|------|
| 00 | Context Router | Always on | Smart-loads KB by task type |
| 01 | Bootstrap | `.ai-context/**` | Detect project type, populate KB |
| 02 | Inventory | `.ai-context/tasks/**` | Structured audit with cross-ref IDs |
| 03 | Proposal | `.ai-context/tasks/**` | Technical proposal, await approval |
| 04 | Implementer | Manual | Code only, delegates tests to 05 |
| 05 | Test Writer | Agent 04 delegation or manual | SOLE owner of test generation |
| 06 | Reviewer | Manual | Quality gate with severity |
| 07 | Meta Optimizer | Multiple | Audit KB, detect drift |

---

## Quick Start

### Prerequisites

- [Cursor IDE](https://cursor.sh) installed
- A project you want to enhance with AI-driven development

### Two Steps

**Step 1: Clone Template**

```bash
cd /path/to/your-project
git clone <template-repo> ai-dev-agents
```

**Step 2: Bootstrap**

Open your project in Cursor, then tell AI:

> "Bootstrap the AI context for this project"

Bootstrap Agent will automatically: detect project type -> copy templates -> scan project -> populate knowledge base -> generate `_meta.yaml` -> generate gap report.

---

## Installation

### Recommended: Git Clone + One Command

```bash
# Step 1: Clone template into your project root
cd /path/to/your-project
git clone https://github.com/your-org/ai-dev-agents.git ai-dev-agents

# Step 2: Open project in Cursor, tell AI:
# "Bootstrap the AI context for this project"
```

### Advanced: Git Subtree

```bash
cd /path/to/your-project

# Add as subtree (first time)
git subtree add --prefix=ai-dev-agents https://github.com/your-org/ai-dev-agents.git main --squash

# Then tell Cursor AI: "Bootstrap the AI context for this project"

# To pull template updates later
git subtree pull --prefix=ai-dev-agents https://github.com/your-org/ai-dev-agents.git main --squash
```

### Verify Installation

After bootstrap, your project should have:

```
your-project/
├── .ai-context/
│   ├── _meta.yaml                 # KB health and completeness tracking
│   ├── project_profile.yaml       # Auto-detected
│   ├── feature_map.yaml           # Auto + PM input
│   ├── contract_registry.yaml     # Auto + Backend input
│   ├── repo_manifest.yaml         # Manual (Infra/Backend)
│   ├── environment_map.yaml       # Auto + Infra input
│   ├── coding_standards.yaml      # Auto + team input (YAML format)
│   ├── domain_glossary.yaml       # Auto + PM input
│   ├── design_system.yaml         # Auto + Designer (FE/fullstack only)
│   ├── data_models.yaml           # Auto + Backend input
│   ├── test_strategy.yaml         # Auto + QA input
│   ├── task_template.yaml         # Ready to use (YAML)
│   ├── task_template.md           # Ready to use (Markdown)
│   └── tasks/
└── .cursor/
    └── rules/
        ├── 00-project-context.mdc
        ├── 01-bootstrap-context.mdc
        ├── 02-inventory.mdc
        ├── 03-proposal.mdc
        ├── 04-implementer.mdc
        ├── 05-test-writer.mdc
        ├── 06-reviewer.mdc
        └── 07-meta-optimizer.mdc
```

---

## Knowledge Base

All knowledge base files are in `.ai-context/` with schema version 2.0.

### Schema v2.0 Features

- **`_meta.yaml`**: Central health tracker with per-file completeness scores
- **Metadata headers**: Every YAML file has a `_meta` section for tracking
- **Cross-reference IDs**: `feat:`, `contract:`, `model:`, `term:` prefixed IDs for linking
- **Conditional applicability**: Files declare which project types they apply to
- **Human-safe updates**: Lines prefixed `# HUMAN:` are never modified by agents

### File Overview

| File | Auto | Owner | Applies To |
|------|:----:|-------|------------|
| `_meta.yaml` | 100% | Bootstrap/Optimizer | All |
| `project_profile.yaml` | 95% | Bootstrap | All |
| `feature_map.yaml` | 60% | PM + Bootstrap | All |
| `contract_registry.yaml` | 80% | Backend | All (except cli) |
| `repo_manifest.yaml` | 10% | Infra/Backend | All (except cli) |
| `environment_map.yaml` | 70% | Infra | FE, BE, Fullstack |
| `coding_standards.yaml` | 50% | Frontend/Backend | All |
| `domain_glossary.yaml` | 20% | PM | All (except cli) |
| `design_system.yaml` | 40% | Designer/Frontend | **Frontend, Fullstack only** |
| `data_models.yaml` | 70% | Backend | **Backend, Fullstack only** |
| `test_strategy.yaml` | 30% | QA/Tech Lead | All |
| `task_template.yaml` | 100% | -- | All |

---

## Agent Reference

### Agent 00: Context Router

- **Trigger**: Always on (`alwaysApply: true`)
- **v2.0 change**: Reads `_meta.yaml` first, then smart-loads only relevant KB files based on task type
- **Loading strategy**: Feature work loads 7 files; bug fix loads 3; test writing loads 3; infra loads 3

### Agent 01: Bootstrap

- **Trigger**: `.ai-context/**`
- **v2.0 change**: Detects `project_type` first (Step 0.5), skips N/A files, generates `_meta.yaml`, logs detection failures

### Agent 02: Inventory (Phase 1)

- **Trigger**: `.ai-context/tasks/**`
- **v2.0 change**: Structured output template with cross-reference IDs, prerequisites gate

### Agent 03: Proposal (Phase 2)

- **Trigger**: `.ai-context/tasks/**`
- **v2.0 change**: Structured output template, delegates test writing to Agent 05, prerequisites gate

### Agent 04: Implementer (Phase 3)

- **Trigger**: Manual
- **v2.0 change**: **Code only** — all test generation delegated to Agent 05, KB update protocol

### Agent 05: Test Writer

- **Trigger**: Agent 04 delegation or manual
- **v2.0 change**: **Sole owner** of all test generation, includes mocking guidelines and flaky test policy

### Agent 06: Reviewer

- **Trigger**: Manual
- **v2.0 change**: Severity levels (CRITICAL/WARNING/SUGGESTION), explicit diff scope, fixed checklist numbering

### Agent 07: Meta Optimizer

- **Trigger**: Multiple (see below)
- **v2.0 change**: Concrete trigger conditions, `_meta.yaml` management, drift detection with evidence

---

## Workflow

### Overview

```
Phase 1: Inventory -> Phase 2: Proposal -> Phase 3: Implementation + Tests -> Review
```

Each phase has **prerequisites** and **gates** to prevent skipping steps.

### Backend Example: Add Search API

```
1. Create task: .ai-context/tasks/add-content-search.yaml
   Fill in: "Add full-text search endpoint for content titles and descriptions"

2. Inventory (Agent 02):
   - affected_features:
     - feat:content-management (HIGH — new endpoint in content module)
   - affected_contracts:
     - contract:rest-v2-admin (additive — new GET endpoint)
   - affected_models:
     - model:content (new_field — add tsvector column)
   - risks:
     - Search performance on large datasets (MEDIUM)
     - DB migration required (LOW — additive change)

3. Proposal (Agent 03):
   - Architecture: Use PostgreSQL tsvector for full-text search
   - Task breakdown:
     Step 1: Add search_vector column migration (schema/20240315.sql)
     Step 2: Add search scope to ContentService (services/content.js)
     Step 3: Add search route + controller (route/api/originals/v2/admin/search.js)
     Step 4: Update Swagger docs
   - Test plan: Unit test for search scope, integration test for API endpoint
   - KB updates: feature_map.yaml, contract_registry.yaml, data_models.yaml

4. Implementation (Agent 04) + Tests (Agent 05):
   - Step 1: Migration file created, schema updated
   - Step 2: ContentService.searchByTitle scope added
   - Step 3: GET /api/v2/admin/content/search?q= endpoint working
   - Agent 05: Unit test + integration test written and passing

5. Review (Agent 06):
   - [PASS] Contract: additive change, no breaks
   - [PASS] Standards: follows existing service pattern
   - [WARNING] Missing index on tsvector column -> fixed
   - Status: PASS after fix
```

### Frontend Example: Add Search Component

```
1. Create task: .ai-context/tasks/add-search-bar.yaml
   Fill in: "Add SearchBar component with debounced input and live results"

2. Inventory (Agent 02):
   - affected_features:
     - feat:content-picker (HIGH — new UI component)
   - affected_contracts:
     - contract:graphql-content-api (additive — new search query)
   - affected_models: [] (no DB changes)

3. Proposal (Agent 03):
   - Architecture: SearchBar with useDebounce hook + React Query
   - Task breakdown:
     Step 1: Create useDebounce hook (src/hooks/useDebounce.ts)
     Step 2: Create SearchBar component (src/components/SearchBar/)
     Step 3: Add searchContents query (src/queries/content/searchContents.ts)
     Step 4: Integrate into ContentPicker

4. Implementation (Agent 04) + Tests (Agent 05)

5. Review (Agent 06)
```

---

## Optimization

### Meta Optimizer Trigger Conditions

| Condition | Description |
|-----------|-------------|
| Manual request | "Run meta optimizer" |
| Task completion | A task reaches status "done" |
| Staleness | `_meta.yaml` → `last_bootstrap` > 30 days old |
| Low completeness | Any file score < 0.5 in `_meta.yaml` |
| Task volume | 5+ tasks since last optimization |

### Maturity Model

| Level | Description | Measurable Criteria |
|-------|-------------|---------------------|
| **L1: Assisted** | AI suggests, human decides everything | KB completeness >= 0.3 |
| **L2: Collaborative** | AI codes, human reviews key decisions | KB completeness >= 0.7, 3+ successful tasks |
| **L3: Autonomous** | AI handles most tasks, human reviews high-risk | Test coverage >= 70%, CI green, 10+ tasks completed |
| **L4: Fully Automated** | AI auto-merges low-risk changes | Zero CRITICAL issues in last 5 reviews |

---

## Supported Stacks

Bootstrap Agent automatically detects `project_type` and adapts:

| Project Type | Signal | Skips |
|-------------|--------|-------|
| `frontend` | React, Vue, Angular, Svelte, Next.js, Nuxt | `data_models.yaml` (if no DB dependency) |
| `backend` | Express, FastAPI, Django, Spring, Gin, NestJS | `design_system.yaml` |
| `fullstack` | Both frontend + backend signals | Nothing skipped |
| `library` | No server, no pages, published package | `design_system.yaml`, `environment_map.yaml` |
| `cli` | CLI entry point, no server | `design_system.yaml`, `environment_map.yaml`, `contract_registry.yaml` |
| `monorepo` | Workspace config (pnpm, Nx, Turborepo) | Per-workspace detection |

### Supported Languages & Frameworks

**Frontend**: React, Next.js, Vue, Nuxt, Angular, Svelte, SvelteKit, Astro  
**Backend**: Node.js (Express, Fastify, NestJS), Python (FastAPI, Django, Flask), Go (Gin, Echo), Java (Spring Boot), C# (.NET), Rust (Actix, Axum)  
**Mobile**: React Native, Flutter, Kotlin, Swift  
**API**: GraphQL, REST/OpenAPI, gRPC, tRPC  
**CI/CD**: GitHub Actions, Azure DevOps, GitLab CI, Jenkins, CircleCI

---

## Troubleshooting

### Wrong Project Type Detected

**Symptom**: `_meta.yaml` → `project_type` is wrong (e.g., "frontend" for a backend project).

**Fix**: Manually edit `_meta.yaml` → `project_type` to the correct value, then re-run Bootstrap:
> "Re-run bootstrap with corrected project type"

### KB File Empty After Bootstrap

**Symptom**: A KB file is still a template with empty fields after bootstrap.

**Check**: Look at `_meta.yaml` → `detection_failures` for errors during that step.

**Fix**: The detection may have failed due to unusual project structure. Fill in manually or tell AI:
> "Please scan [specific directory] and populate [specific file]"

### Agent Not Triggering

**Symptom**: You opened a task file but the Inventory Agent didn't activate.

**Check**: Ensure the file is in `.ai-context/tasks/` (correct path), and `.cursor/rules/02-inventory.mdc` exists.

**Fix**: Explicitly invoke the agent:
> "Run inventory for the task in .ai-context/tasks/my-task.yaml"

### Stale Knowledge Base After Major Refactor

**Symptom**: KB files reference files/patterns that no longer exist.

**Fix**:
1. Run Meta Optimizer to detect drift: "Run meta optimizer"
2. If drift is extensive, re-run Bootstrap: "Re-bootstrap the AI context"
3. Review and merge Bootstrap output with existing human-provided content

### Corrupted _meta.yaml

**Symptom**: `_meta.yaml` has invalid YAML or wrong scores.

**Fix**: Delete `_meta.yaml` and run Meta Optimizer — it will regenerate from scratch:
> "Regenerate _meta.yaml by auditing the knowledge base"

---

## FAQ

### Q: How is this different from GitHub Copilot / Cursor's built-in features?

**A**: Copilot and Cursor only see the current file and limited context. This project provides **structured project-wide knowledge** (feature mapping, API contracts, environment configuration, cross-references) plus **workflow control** (Inventory -> Proposal -> Implementation), making AI behavior predictable, reliable, and consistent.

### Q: How long does initialization take?

**A**: Bootstrap's automatic scan takes about 1-2 minutes. Manual supplementation of business rules and cross-repo information takes 30 minutes to 2 hours, depending on project complexity.

### Q: Should knowledge base files be committed to Git?

**A**: Yes, highly recommended. The knowledge base is a team-shared asset and should be version controlled.

### Q: Can I use only some of the Agents?

**A**: Yes. `00-project-context.mdc` and `01-bootstrap-context.mdc` are foundational. Other Agents can be enabled as needed.

### Q: Does this support monorepos?

**A**: Yes. Bootstrap will detect workspace configuration and set `project_type` to `monorepo`.

### Q: Why was coding_standards.md changed to YAML?

**A**: v2.0 changed `coding_standards.md` to `coding_standards.yaml` so Agents can reliably parse naming conventions, lint settings, and pattern definitions. Human readability is maintained through clear YAML structure and comments.

### Q: What are cross-reference IDs?

**A**: Each feature, contract, data model, and term has a stable ID (e.g., `feat:user-auth`, `contract:rest-v2-admin`, `model:content`, `term:jwt-token`). This allows different KB files to reference each other, enabling Agents to track cross-file relationships.

---

## Contributing

Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for details on how to contribute.

---

## License

MIT License. See [LICENSE](./LICENSE) for details.

```
MIT License

Copyright (c) 2025-present

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
