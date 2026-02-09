# AI Context Knowledge Base

This directory contains the structured knowledge base that powers AI-driven development in this project.

## Schema Version: 2.0

All YAML files follow a consistent structure with `_meta` headers for tracking completeness, staleness, and project-type applicability.

## How It Works

AI agents (`.cursor/rules/`) read these files to understand your project before making any changes. The Bootstrap Agent (`01-bootstrap-context.mdc`) auto-populates most files by scanning your project, but some fields require human input (marked with `# HUMAN:` or `# TODO`).

The Context Router (`00-project-context.mdc`) reads `_meta.yaml` first to determine project type and selectively loads only relevant KB files.

## Files

| File | Auto | Owner | Purpose |
|------|:----:|-------|---------|
| `_meta.yaml` | 100% | Bootstrap | KB health: schema version, completeness scores, gaps by owner |
| `project_profile.yaml` | 95% | Bootstrap | Project tech fingerprint: languages, frameworks, tools, structure |
| `feature_map.yaml` | 60% | PM + Bootstrap | Maps business features to code locations with cross-references |
| `contract_registry.yaml` | 80% | Backend + Bootstrap | All API contracts produced or consumed, with versioning |
| `repo_manifest.yaml` | 10% | Infra/Backend | Multi-repo ecosystem: repos, communication patterns, deploy coordination |
| `environment_map.yaml` | 70% | Infra + Bootstrap | Environment configs: endpoints, DB, cache, secrets, SLOs |
| `coding_standards.yaml` | 50% | Frontend/Backend | Team coding conventions, patterns, lint rules (YAML for machine parsing) |
| `domain_glossary.yaml` | 20% | PM | Domain terms, roles with structured permissions, business entity lifecycle |
| `design_system.yaml` | 40% | Designer/Frontend | Design tokens, components, accessibility (**frontend/fullstack only**) |
| `data_models.yaml` | 70% | Backend | Database schema, entity relations, indexes, query patterns |
| `test_strategy.yaml` | 30% | QA/Tech Lead | Test levels, quality gates, mocking guidelines, flaky test policy |
| `task_template.yaml` | 100% | — | YAML task template for AI agent consumption |
| `task_template.md` | 100% | — | Markdown task template for human readability |

## Cross-Referencing

Files use stable IDs to link to each other:

- **Features**: `feat:content-management` (in `feature_map.yaml`)
- **Contracts**: `contract:rest-v2-admin` (in `contract_registry.yaml`)
- **Models**: `model:content` (in `data_models.yaml`)
- **Terms**: `term:original-content` (in `domain_glossary.yaml`)

Example: A feature entry can reference its contracts, models, and terms:

```yaml
- id: "feat:content-management"
  contracts: ["contract:rest-v2-admin"]
  models: ["model:content", "model:tags"]
  terms: ["term:original-content"]
```

## Conditional Applicability

Each file declares which project types it applies to via `_meta.applicable_when.project_type`. Files not applicable to the current project type are skipped by Bootstrap and Context Router.

For example, `design_system.yaml` is skipped for `backend` and `library` projects.

## Directory: `tasks/`

Contains active and completed task files. Each task follows the three-phase workflow:

1. **Inventory**: Context audit, impact analysis (Agent 02)
2. **Proposal**: Technical design, task breakdown, test plan (Agent 03)
3. **Implementation**: Step-by-step execution (Agent 04 + 05)

## Getting Started

1. Run the Bootstrap Agent to auto-populate files:
   > "初始化此專案的 AI 上下文" / "Bootstrap the AI context for this project"

2. Review auto-generated content and fill in human-required fields:
   - **PM**: `domain_glossary.yaml` — terms, roles, permissions, business rules
   - **Designer**: `design_system.yaml` — tokens, components, accessibility
   - **Backend**: `data_models.yaml` — verify schema, add business validation
   - **QA**: `test_strategy.yaml` — coverage targets, quality gates, baselines

3. Start your first task by copying `task_template.yaml` to `tasks/your-task.yaml`

## Maintenance

- Update files when the project changes (new features, API changes, schema changes)
- Run the Meta Optimizer Agent periodically to detect drift
- Lines prefixed with `# HUMAN:` are never overwritten by agents
- All files should be version-controlled (committed to Git)
