# Resource Inventory and Environment Topology

This page records verified resources and defines how Dev, Test, and Prod environments should be registered and isolated. The **current-state scope** is the Azure subscription signed in on 12 October 2026 and the repository configuration. It is not a complete inventory of Balco's servers or production assets.

## Azure Resources

Resource group: `balco-poc`

| Resource | Azure type | Purpose | Owner |
| --- | --- | --- | --- |
| `balco-poc` | Azure AI Services and project | AI-related POC resources | Su |
| `balco-poc-web` | Static Web Apps | POC frontend; default domain `mango-grass-04a587310.2.azurestaticapps.net` | Su |
| `balco-poc-api` | App Service | POC .NET API; current state Running, `httpsOnly=true` | Su |
| `balco` | Azure Container Registry | Image registry; see the Dev section for usage rules | Su |
| `weekly-report` | Container Apps Job | Weekly report task | Su |
| `balco-translator` | Azure AI service | Translation service resource | Su |

## Physical and Virtual Machines

| Hostname | IP | Environment | Username | User |
| --- | --- | --- | --- | --- |
| `dev-01` | xxx | Dev | `balcoadmin` | Jason, Su |
| `dev-01` | xxx | Dev | `balco01` | Su |
| `dev-01` | xxx | Dev | `balco02` | |
| `dev-01` | xxx | Dev | `balco03` | |
| `test-01` | xxx | Test | `balcoadmin` | Jason, Su |
| `test-01` | xxx | Test | `balco01` | Su |
| `test-01` | xxx | Test | `balco02` | |
| `test-01` | xxx | Test | `balco03` | |

## Asset Registration Rules

Record the `resource group, type, resource, environment, system, and owner` for each resource. Update the record when a resource is created or changed, and reconcile it against an Azure export monthly. Add host details for non-Azure servers.

## TODO

- Update Azure status after migration from a personal account to a company account.
- Update the physical and virtual machine inventory when those resources are ready.
