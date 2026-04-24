# Repo Standards -- Accelerate Data

Canonical reference for CI/CD, branching, testing, security, and governance
across all Accelerate Data repositories. Every new repo must comply with these
standards before its first production merge.

---

## Table of Contents

1. [CI/CD Baseline](#1-cicd-baseline)
2. [Branch Rulesets](#2-branch-rulesets)
3. [Branch Naming and Strategy](#3-branch-naming-and-strategy)
4. [Documentation](#4-documentation)
5. [AI Agent Integration](#5-ai-agent-integration)
6. [Code Quality](#6-code-quality)
7. [Testing](#7-testing)
8. [Security](#8-security)
9. [Governance](#9-governance)
10. [Repo Initialization Checklist](#10-repo-initialization-checklist)

---

## 1. CI/CD Baseline

### Required Automation (every repo)

| File | Trigger | Purpose |
| --- | --- | --- |
| `ci.yml` | Push to `main`, PR to `main` | Lint, typecheck, unit tests |
| `claude.yml` | PR events | Claude Code integration |
| `claude-code-review.yml` | PR opened/updated | AI-powered PR review |
| `codeql.yml` | Push to `main`, PR to `main`, weekly schedule | Static analysis (SAST) |
| `dependency-review.yml` | PR to `main` | Block PRs that introduce vulnerable dependencies |
| `release.yml` | Push tag `v*` | Semver release with changelog generation |
| `.github/dependabot.yml` | Dependabot schedule | Automated dependency updates |

Dependabot PRs that pass CI are auto-merged for patch and minor updates.

### Optional Workflows (by repo type)

| Workflow | When to Add |
| --- | --- |
| `docs.yml` | Repos with a docs site (VitePress, Docusaurus) |
| `container-scan.yml` | Repos that build container images |
| `nightly-cleanup.yml` | Repos with generated artifacts or temp resources |

### CI/CD Rules

- CI runs on **push to `main`** and **PR to `main`**.
- All CI checks **gate merge** -- a PR cannot merge with failing checks.
- The `engineering-skills` plugin provides the `adversarial-review` skill for
  deeper AI-assisted review on critical paths.

---

## 2. Branch Rulesets

The `main` branch is protected via a GitHub repository ruleset. The ruleset named **`main`** targets `refs/heads/main` and is set to **active** enforcement.

### Rules

| Rule | Configuration |
| --- | --- |
| Deletion | Blocked |
| Force push | Blocked (non-fast-forward) |
| Pull request required | Yes — 1 required approving review |
| Dismiss stale reviews on push | Yes |
| Require code owner review | Yes |
| Require last push approval | No |
| Allowed merge methods | Squash |
| Required status checks (strict) | `claude-review`, `dependency-review`, `codeql` |
| Code scanning | CodeQL — high-or-higher security alerts, errors threshold |
| Code quality | Errors severity |

### Bypass

Organization admins are exempt from all rules.

No other bypass actors. If a hotfix must land quickly, it still requires a PR
and one admin approval — an org admin can merge without waiting for status
checks if necessary.

---

## 3. Branch Naming and Strategy

### Branch Prefixes

| Prefix | Use Case | Example |
| --- | --- | --- |
| `feature/` | New functionality | `feature/VD-123-pipeline-builder` |
| `fix/` | Bug fixes | `fix/VD-456-null-pointer-transform` |
| `chore/` | Maintenance, config | `chore/upgrade-node-22` |
| `docs/` | Documentation only | `docs/api-reference-update` |
| `refactor/` | Code restructuring | `refactor/extract-auth-module` |
| `dependabot/*` | Automated dependency PRs | `dependabot/npm/lodash-4.17.21` |
| `release/` | Release preparation | `release/v2.1.0` |

### Doc Branches

Branches prefixed with `docs/` follow a lighter CI path:

- Skip unit/integration tests.
- Run only markdown lint and docs build.
- Require one review (can be AI-only for typo fixes).

### Worktree-First Workflow

Never switch branches on the main working copy. Every feature gets its own worktree — an isolated copy of the codebase with its own branch, files, and environment.

For setup commands, path conventions, parallel execution patterns, and
lifecycle management, see the canonical guide:
[guides/worktree-workflow.md](https://github.com/accelerate-data/engineering-framework/blob/main/guides/worktree-workflow.md).

### PR Format

- **Title**: `VD-XXX: short description` (e.g., `VD-142: add retry logic to HTTP transform`)
- **Body**: Must include `Fixes VD-XXX` to auto-link the Linear ticket.

### Branch Lifecycle

- Branches auto-delete after merge.
- Branches older than 30 days without activity are flagged for cleanup.

---

## 4. Documentation

### Required Files (every repo)

| File | Purpose |
| --- | --- |
| `README.md` | Project overview, setup, usage |
| `CLAUDE.md` | AI agent instructions and constraints |
| `AGENTS.md` | Agent definitions and capabilities |
| `CONTRIBUTING.md` | Contribution workflow and standards |
| `SECURITY.md` | Vulnerability reporting process |
| `.github/CODEOWNERS` | Ownership rules for code review |
| `.github/pull_request_template.md` | PR template |
| `.github/ISSUE_TEMPLATE/` | Issue templates (bug, feature, task) |

### Documentation Directory Structure

```text
docs/
  design/          # Architecture and design decision records
  functional/      # Functional specifications
  plans/           # Implementation plans
  runbooks/        # Operational procedures
  user-guide/      # End-user documentation
  reference/       # API reference, configuration reference
```

### README-First Principle

Every directory must have a README explaining what it contains and why. If
someone cannot understand a directory from its README alone, the documentation
is incomplete.

### Docs Site

Use VitePress or GitHub Wiki sync for repos that need a browsable
documentation site. Source files live in `docs/` and deploy via `docs.yml`.

### Style

- Plain language, active voice, no idioms.
- Design docs follow: **Context > Decision > Consequences**.
- Specifications use **EARS requirements syntax**.
- Stakeholder review packages use the `reporting-skills` plugin's
  `package-for-review` skill.

### Plugins

| Plugin | Skills |
| --- | --- |
| `doc-skills` | `create-spec`, `write-design-doc`, `write-user-guide` |
| `reporting-skills` | `package-for-review` |

---

## 5. AI Agent Integration

### Governance as Code

Governance files are **active constraints**, not passive documentation. Every
AI agent reads and enforces them at startup. Treat changes to governance files
with the same rigor as code changes.

### Required `.claude/` Structure

```text
.claude/
  settings.json        # Tool permissions, MCP config
  rules/
    frontend.md        # Frontend coding standards
    backend.md         # Backend coding standards
    git-workflow.md    # Git and PR conventions
  commands/            # Slash commands
  skills/              # Reusable skill definitions
  agents/              # Agent role definitions
```

### Required Root Files

| File | Required When |
| --- | --- |
| `CLAUDE.md` | Always |
| `AGENTS.md` | Always |
| `repo-map.json` | Repos with 50+ files |
| `.mcp.json` | Repos using MCP servers |

### Linear Lifecycle

Every change follows the Linear ticket lifecycle:

1. `/creating-linear-issue` -- create the ticket
2. `/implementing-linear-issue VD-XXX` -- implement against the ticket
3. `/raising-linear-pr VD-XXX` — raise PR for the issue
4. `/closing-linear-issue VD-XXX` — close linear issue

### Plugin Matrix

| Scope | Plugin | Skills |
| --- | --- | --- |
| All repos | `engineering-skills` | `creating-linear-issue`, `implementing-linear-issue`, `raising-linear-pr`, `closing-linear-issue`, `adversarial-review` |
| AI/LLM repos | `promptfoo` | `eval`, `regression`, `red-teaming` |
| Backend repos | `backend-skills` | Coding standards enforcement |
| Frontend repos | `frontend-skills` | React, shadcn/ui conventions |
| All repos | `superpowers` | TDD workflow, debugging |
| Review workflows | `reporting-skills` | `package-for-review` |

Accelerate Data's own skills are packaged and maintained in the
[engineering-framework](https://github.com/accelerate-data/engineering-framework)
repository. The
[plugin-marketplace](https://github.com/accelerate-data/plugin-marketplace)
lists all available plugins.

---

## 6. Code Quality

### Tooling by Language

| Tool Category | TypeScript | Python |
| --- | --- | --- |
| Linter | ESLint (flat config) | Ruff |
| Formatter | Prettier | Ruff / Black |
| Type checker | `tsc --noEmit` | mypy / pyright |
| Pre-commit | Husky + lint-staged | `.githooks/pre-commit` |
| Markdown lint | markdownlint | markdownlint |

### Code Quality Rules

Every repo must have:

- A **formatter** configured and enforced.
- A **linter** with zero warnings in CI.
- **Pre-commit hooks** that run formatting and linting before each commit.
- An `.editorconfig` file for consistent whitespace and encoding.

---

## 7. Testing

### Test Tiers

| Tier | Requirement | Scope |
| --- | --- | --- |
| Unit | Required | All repos |
| Integration | Required | Backend and API repos |
| E2E (Playwright) | Recommended | Repos with a UI |
| BDD | Optional | Complex domain logic |
| Agent evals (promptfoo) | Required | AI/LLM repos |

### Coverage

- Minimum **70% line coverage** enforced in CI.
- Coverage reports uploaded as PR comments.

### Test Manifest

Complex repos should include a `TEST_MANIFEST.md` documenting:

- What each test suite covers.
- How to run tests locally.
- Known gaps and planned coverage improvements.

---

## 8. Security

### Required for All Repos

| Item | Purpose |
| --- | --- |
| `codeql.yml` | Static analysis on every PR and weekly |
| `dependency-review.yml` | Block vulnerable dependency introductions |
| `.github/dependabot.yml` + auto-merge | Keep dependencies current |
| `.gitleaks.toml` | Prevent secret leaks in commits |
| `.env.example` | Document required env vars (never commit `.env`) |
| `SECURITY.md` | Vulnerability disclosure process |
| `.github/prompts/security-review.md` | Security review prompt for AI agents |

### Container Repos (additional)

| Item | Purpose |
| --- | --- |
| `container-scan.yml` | Scan images for vulnerabilities on build |
| `.semgrepignore` | Semgrep exclusion rules |

### Security Rules

- Never commit `.env` files. Use `.env.example` with placeholder values.
- Secrets must be stored in GitHub Secrets or a vault, never in code.
- Dependabot auto-merge applies only to patch and minor updates that pass CI.

---

## 9. Governance

### Code Ownership

Every repo must have a `.github/CODEOWNERS` file with at minimum:

```text
* @accelerate-data/vibedata-engineering
```

Add path-specific owners for specialized areas:

```text
/src/auth/    @admiraldata
/docs/        @accelerate-data/vibedata-engineering
/infra/       @accelerate-data/vibedata-engineering
```

### PR Process

1. Every change requires a **pull request** -- no direct pushes to `main`.
2. PR must link a **Linear ticket** (`Fixes VD-XXX` in the body).
3. AI review (`claude-code-review.yml`) runs automatically.
4. At least **one human review** is required.
5. Merge strategy: **squash merge by default** to keep `main` history clean.

### Release Process

- Follow **semantic versioning** (semver).
- Changelogs are generated from **conventional commit** messages.
- Releases are tagged as `vX.Y.Z` and trigger `release.yml`.

---

## 10. Repo Initialization Checklist

Use this checklist when creating a new repo or auditing an existing one.

- [ ] Branch ruleset `main` created and active (per Section 2)
- [ ] Auto-delete branches after merge enabled
- [ ] `.github/dependabot.yml` configured and auto-merge enabled
- [ ] Required files present: `README.md`, `CLAUDE.md`, `AGENTS.md`,
      `CONTRIBUTING.md`, `SECURITY.md`
- [ ] `.github/CODEOWNERS` created with minimum ownership rule
- [ ] `.claude/` structure created (per Section 5)
- [ ] CI workflows added: `ci.yml`, `claude.yml`, `claude-code-review.yml`,
      `codeql.yml`, `dependency-review.yml`, `release.yml`
- [ ] Linter, formatter, and pre-commit hooks configured (per Section 6)
- [ ] `.gitleaks.toml` added and `.env.example` created
- [ ] Marketplace plugins referenced in `CLAUDE.md` (per Section 5 matrix)
