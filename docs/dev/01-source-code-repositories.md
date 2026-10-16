# Source Repositories, Access, and Structure

Balco manages code, documentation, and pipeline configuration in **GitHub**. New projects should be created in a company-managed GitHub Organization. Repository ownership, administrators, and business owners must remain transferable after staff departures.

## Repository Boundaries

- Use one main repository for each independently released application or service. Frontend and backend components released and versioned together may share a repository.
- Name new repositories `<system>-<component>` or `<system>`, for example `ForageAI-api` or `ForageAI`.
- Create private repositories by default. Before publishing source code or documentation, review data, secrets, licenses, and the intended publication scope.
- Do not store production data exports, personal information, certificate private keys, access tokens, or container image binaries in Git. Store images in ACR; see [artifact management](./02-image-and-artifact-registry.md).

## Minimum Directory Structure

```text
.github/workflows/      # CI/CD pipelines
src/                    # application or service source code
tests/                  # automated tests
docs/                   # architecture, design, APIs, runbooks
README.md               # purpose, local setup, deployment entry points
.env.example            # examples of non-sensitive configuration
```

Adapt the directory structure to the actual language or framework.

## Role Permissions

| Role | GitHub permission | Responsibility |
| --- | --- | --- |
| Platform administrator | Admin | Repository settings, rules, emergency recovery |
| Project maintainer | Maintain | Reviews, versioning, release coordination |
| Developer | Write | Branch development and pull requests |
| Reviewer or external collaborator | Read or as needed | Viewing, reviewing, or limited-scope collaboration |

## TODO

- Create a company account and migrate POC GitHub repositories to it.
