# .github-draft -- Accelerate Data Org Defaults

This repository contains **org-wide standards, templates, and defaults** for all
[Accelerate Data](https://github.com/accelerate-data) repositories.

## Status: Draft

This repo is named `.github-draft` so it has **no effect** on the organization.
Once reviewed and approved, rename it to `.github` to activate it.

When activated, GitHub automatically applies its contents as defaults to every
org repo that does not define its own:

- **Issue templates** and **PR template** -- consistent contribution workflow
- **CONTRIBUTING.md** -- contribution guidelines
- **SECURITY.md** -- vulnerability reporting process
- **Org profile** (`profile/README.md`) -- displayed on the org's GitHub page

## Contents

| Path | Purpose |
|------|---------|
| `REPO-STANDARDS.md` | Canonical reference for CI, branching, testing, security, and governance |
| `.github/` | PR template, issue templates, workflow references |
| `profile/README.md` | Public org profile shown at github.com/accelerate-data |

## How to Review

1. Read **REPO-STANDARDS.md** first -- it defines every standard.
2. Check templates in `.github/` for consistency with the standards.
3. Review `profile/README.md` for the public-facing org description.

## How to Activate

1. Go to the repo's **Settings > General > Repository name**.
2. Rename from `.github-draft` to `.github`.
3. Confirm the rename. GitHub begins applying defaults immediately.

## Owners

| GitHub Handle | Name |
|---------------|------|
| @admiraldata | Shwetank Sheel |
| @hbanerjee74 | Hemanta Banerjee |
| @ukakkad | Umesh Kakkad |
