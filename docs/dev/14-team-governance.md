# Team Responsibilities, Standards Exceptions, Reviews, and Updates

Every application and shared platform needs an explicit owner. Owners maintain their documentation, gates, and release records; accountability must remain assigned even when someone else performs the implementation.

## Responsibilities

| Role | Main responsibility |
| --- | --- |
| Business or product owner | Set requirement priorities, acceptance criteria, and formal release windows |
| Technical lead | Architecture and code quality, critical PR reviews, technical debt, and version compatibility |
| Developer | Implementation, testing, documentation updates, PR self-review, and defect fixes |
| Test or acceptance owner | Maintain test scope, Test acceptance, and defect conclusions |
| Platform and operations owner | GitHub, ACR, shared environments, deployment access, monitoring, and recovery exercises |
| Security owner | Assess identity, secrets, dependency risk, and security exceptions |

In a small team, one person may hold several roles, but a second person should participate in production release approvals and reviews of critical changes. When people change roles, transfer systems, access, open tasks, and on-call details under the [onboarding and offboarding process](./04-access-and-onboarding.md).

## Standards Changes

After these standards are added to the company GitHub repository, change them through PRs. Proposals should explain background, scope of impact, migration steps, and effective date. Relevant application, platform, or security owners review them. Validate breaking changes in Dev/Test before notifying affected teams.
