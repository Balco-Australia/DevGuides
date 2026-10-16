# Branches, Commits, and Contributions

Balco collaborates through GitHub. Every change should be traceable from a requirement or defect to a branch, commit, pull request (PR), and release. See [source repository standards](./01-source-code-repositories.md) for access and structure.

## Branch Rules

| Branch | Purpose | Merge direction |
| --- | --- | --- |
| `main` | Main branch holding verified, releasable code | Accepts PRs only from `dev` |
| `dev` | Development integration branch collecting completed work | Accepts PRs from working branches; opens PRs to `main` after validation |
| `dev-xxx` | Personal development branch | Merges into `dev` when complete |
| `feat-xxx` | New feature | Merges into `dev` when complete |
| `hotfix-xxx` | Urgent defect fix | Merges into `dev` when complete |
| `docs-xxx` | Documentation change | Merges into `dev` when complete |

Developers create working branches from `dev` and use a short description for `xxx`, such as `feat-report` or `dev-su`. Keep working branches short-lived and delete them after merging. For further work, branch from or synchronize with the latest `dev`. Protect both `main` and `dev` from direct pushes. The normal flow is **working branch → dev → main**.

## Commit Rules

Each commit addresses one clear purpose. Use `type(optional scope): short description`, with a module or component as the optional scope. Choose the type that best matches the change:

| Type | Applicable change | Example |
| --- | --- | --- |
| `feat` | New feature or user-visible capability | `feat: add export feature` |
| `fix` | Defect fix | `fix: resolve sign-in failure` |
| `docs` | Documentation only | `docs: expand authentication guidance` |
| `refactor` | Code structure without external behavior change | `refactor: split order service` |
| `test` | Add or adjust tests without changing business code | `test: cover export flow` |
| `perf` | Performance improvement | `perf: reduce duplicate queries` |
| `build` | Dependency, build, or packaging change | `build: pin Bun version` |
| `ci` | CI or deployment pipeline change | `ci: add test step` |
| `chore` | Other maintenance without functional impact | `chore: update code owners` |

If a change corresponds to a GitHub Issue, include its number at the end of the subject or in the body, for example `feat: add export feature (#123)`. List multiple Issues in the body. Do not invent an Issue number when none exists. Describe the actual change instead of writing only “update” or “WIP”. Explain impact and migration in the commit body for incompatible changes.

Review the change scope before committing. Do not add passwords, tokens, certificate private keys, production data, or build outputs containing internal information to Git history. If credentials are committed accidentally, revoke them immediately and follow the [secrets standard](./07-configuration-and-secrets-platform.md). Deleting the file in a later commit does not remove the exposure from history.

## Contribution and Review Rules

1. Developers complete code, tests, and related documentation on a working branch, run the required local checks, then push and open a PR to `dev`.
2. The PR description links the work item and explains the change scope, validation results, configuration or data-migration impact, and rollback approach. Include screenshots for UI changes and compatibility notes for API or database changes.
3. Authors review their own work and wait for required checks. At least one person other than the author reviews it. The relevant owner reviews critical configuration, permissions, infrastructure, and pipeline changes; `CODEOWNERS` can identify review teams. Resolve blocking comments before merging.
4. After `dev` integration validation, the release owner opens a `dev → main` PR, documenting the planned version, Test acceptance results, and release impact. Merge after review and required checks, then release or roll back according to [version and artifact traceability](./10-version-and-artifact-policy.md).

Protect `dev` and `main` with separate GitHub Rulesets that restrict direct pushes and require PR reviews, status checks, and resolved conversations.

## TODO

- Add diagrams.
- Configure branch protection rules in GitHub.
