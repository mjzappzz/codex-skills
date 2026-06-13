# Codex Skills

This repository stores local Codex skill definitions used by this environment.

Each skill is a self-contained directory with a `SKILL.md` file. Codex reads the skill metadata and instructions from that file when a task explicitly routes to a skill.

## Directory Layout

```text
.
├── <skill-name>/
│   └── SKILL.md
├── .system/              # system-managed skills, keep local unless intentionally publishing
└── README.md
```

Common optional subdirectories inside a skill:

- `scripts/`: reusable helper scripts.
- `references/`: task-specific reference material loaded on demand.
- `assets/`: templates, examples, or static assets reused by the skill.

## Usage

Place this directory at:

```text
~/.codex/skills
```

A skill is available when it contains:

```text
<skill-name>/SKILL.md
```

The `SKILL.md` file should start with YAML frontmatter:

```yaml
---
name: skill-name
description: Short description of when to use this skill.
---
```

## Available Skills

| Skill | Purpose |
| --- | --- |
| `api-and-interface-design` | Design stable APIs, module boundaries, and interface contracts. |
| `browser-testing-with-devtools` | Verify browser behavior with Chrome DevTools MCP. |
| `ci-cd-and-automation` | Set up and maintain CI/CD quality gates and automation. |
| `code-review-and-quality` | Review code for correctness, maintainability, security, and performance. |
| `code-simplification` | Refactor code for clarity without changing behavior. |
| `context-engineering` | Curate project context and agent instructions. |
| `debugging-and-error-recovery` | Diagnose failures from actual errors and recover safely. |
| `deprecation-and-migration` | Plan and execute migrations, removals, and sunsets. |
| `documentation-and-adrs` | Capture decisions, architecture notes, and durable documentation. |
| `doubt-driven-development` | Stress-check non-trivial decisions with adversarial review. |
| `frontend-ui-engineering` | Build and review production-quality user interfaces. |
| `git-workflow-and-versioning` | Manage branch, commit, and version-control workflows. |
| `idea-refine` | Refine vague ideas into actionable concepts. |
| `incremental-implementation` | Deliver multi-step changes in small verified increments. |
| `interview-me` | Clarify ambiguous requests through focused questioning. |
| `observability-and-instrumentation` | Add logs, metrics, traces, alerts, and operational visibility. |
| `performance-optimization` | Profile, identify, and fix measured performance bottlenecks. |
| `planning-and-task-breakdown` | Break clear requirements into ordered implementation tasks. |
| `security-and-hardening` | Harden code paths involving input, auth, data, and integrations. |
| `shipping-and-launch` | Prepare releases with rollout, monitoring, and rollback plans. |
| `source-driven-development` | Ground framework/library decisions in official sources. |
| `spec-driven-development` | Write structured specs before significant implementation work. |
| `stress-report` | Generate evidence-based server stress test reports. |
| `test-driven-development` | Prove behavior changes with tests. |
| `ui-ux-pro-max` | Provide UI/UX design guidance across common frontend stacks. |
| `using-agent-skills` | Discover and route work to the appropriate skill. |

## Maintenance

- Keep each skill focused on one workflow or domain.
- Put the trigger conditions in the `description` frontmatter.
- Store long examples, scripts, and references outside `SKILL.md` and load them only when needed.
- Avoid committing secrets, tokens, private keys, license files, customer data, or machine-specific runtime output.
- Before pushing, check for unexpected local-only files:

```bash
find . -maxdepth 3 -type f \
  ! -path './.git/*' \
  ! -path './.codex/*' \
  ! -path './.agents/*' \
  | sort
```

## Publishing Notes

If this directory is pushed to a remote repository, review whether `.system/`, `.codex/`, `.agents/`, generated caches, and local report outputs should be excluded with `.gitignore`.

Recommended first-time repository setup:

```bash
git init
git add README.md */SKILL.md
git commit -m "Add Codex skills"
git remote add origin <repo-url>
git push -u origin main
```
