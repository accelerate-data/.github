# Contributing to Accelerate Data

Thank you for contributing. This guide covers the workflow and standards for all repositories in the `accelerate-data` organization.

## Getting Started

1. Clone the repository and install dependencies per its README.
2. Create a branch from `main` using the naming conventions below.
3. Make your changes, push, and open a pull request.

## Branch Naming

| Prefix | Use |
| --- | --- |
| `feature/` | New functionality, e.g. `feature/VD-123-pipeline-builder` |
| `fix/` | Bug fixes, e.g. `fix/VD-456-null-pointer-transform` |
| `chore/` | Tooling, CI, dependencies, e.g. `chore/upgrade-node-22` |
| `docs/` | Documentation only, e.g. `docs/api-reference-update` |
| `refactor/` | Code restructuring with no behavior change, e.g. `refactor/extract-auth-module` |
| `dependabot/*` | Automated dependency PRs |
| `release/` | Release preparation, e.g. `release/v2.1.0` |

## Worktree Workflow

Never switch branches on the main repository checkout. Every feature gets its own git worktree — an isolated copy of the codebase with its own branch, files, and environment.

See the full guide:
[guides/worktree-workflow.md](https://github.com/accelerate-data/engineering-framework/blob/main/guides/worktree-workflow.md).

## Linear-Driven Lifecycle

All work is tracked in Linear. The typical flow:

1. **Create** a Linear issue (`/creating-linear-issue`) describing the work.
2. **Implement** the issue (`/implementing-linear-issue VD-XXX`), referencing it in commits and PRs.
3. **Raise** the pull request (`/raising-linear-pr VD-XXX`) after implementation is complete.
4. **Close** the issue (`/closing-linear-issue VD-XXX`) after the PR merges.

## Pull Request Requirements

- **Title format**: `VD-XXX: Short description of change`
- **Body**: Include `Fixes VD-XXX` to auto-link the Linear ticket.
- **CI**: All required status checks must pass: `claude-review`, `dependency-review`, and `codeql`.
- **Review**: Requires AI review plus at least 1 human approval.
- **Merge strategy**: Squash merge to `main` by default.

## Code Standards

- Follow the linting and formatting rules configured in the repository.
- Write tests for new functionality. Maintain or improve existing coverage.
- Do not commit secrets, credentials, or `.env` files.

## Documentation

Follow the README-first principle: update the README before or alongside code changes. For design documents, specifications, and proposals, follow the documentation workflow in `REPO-STANDARDS.md`.

## Marketplace Plugins

For plugins and extensions, see the [Plugin Marketplace](https://github.com/accelerate-data/plugin-marketplace).

## Questions

Open an issue or reach out to the maintainers if anything is unclear.
