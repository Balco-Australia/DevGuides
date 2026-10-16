# Developer Access, Role Changes, and Offboarding

Engineering access uses **individual identities, role-based grants, and regular reviews**. Provisioning and removal cover GitHub, Azure/ACR, development hosts, gateway configuration, the monitoring platform, and application test environments. Separate shared-platform administrator access from application development access.

## Onboarding

1. The line manager submits the person's name, role, project, start date, and expected end date. The system owner confirms required resources.
2. IT enables the company identity and multifactor authentication. Add the person to the appropriate GitHub teams instead of broadly granting access repository by repository.
3. The platform owner grants Azure access by environment. Developers receive read-only or required operational access to Dev resources by default. ACR access defaults to pull; CI identities perform pushes.
4. If an Ubuntu host is needed, assign a unique `balcoNN` account and register its user mapping; see the [server baseline](/infrastructure/04-server-baseline).
5. After the first sign-in, verify access to repositories, the Dev environment, logs, and development tools. Record the access request and approver.

## Permission Matrix

| Identity | GitHub | Azure/ACR | Prod |
| --- | --- | --- | --- |
| Developer | Write on project repositories | Required Dev access; ACR Read/Pull | No direct deployment rights by default |
| Project maintainer | Maintain on project repositories | Required Dev/Test access | Release through an approved pipeline |
| Platform administrator | Admin on platform repositories and settings | Manage platform resources | Record emergency operations |
| External collaborator | Minimum access to specified repositories or projects | None by default | None |

## Role Changes and Departures

When a role changes, remove access to the old project and environment before granting new access. On the day a departure notice arrives, revoke GitHub organization and repository access, Azure roles, ACR permissions, VPN/host accounts, vault access, and monitoring-platform access. Inventory personal PATs, SSH keys, app-registration credentials, and automation jobs owned by the person. Transfer repository ownership, alert contacts, and incomplete releases.

Each quarter, the platform owner exports a member and role list for project owners to confirm continuing access. Track anomalous permissions, ownerless service identities, and expired external collaborators for cleanup.
