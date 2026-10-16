# Storage, Backup, and Retention

Set backup policy according to **data importance and recovery objectives**, not merely whether Azure manages a resource. The current `balco-poc` has App Service, Static Web Apps, Log Analytics, Container Registry, and Container Apps Job resources. Their configuration, images, logs, and business files require different recovery methods.

## Recovery Materials by Resource Type

| Resource | Content to retain or rebuild | Minimum validation |
| --- | --- | --- |
| Frontend and API | Source code, build outputs, deployment configuration, environment variable names | Rebuild from the repository and deploy to a non-production environment |
| Azure resources | IaC, resource parameters, access, and network settings | Validate recreation with `what-if` or an equivalent method |
| Container jobs | Image digest, job definition, input and output files | Complete one recovery run using a sample task |
| Business data | Production data in SharePoint, databases, or file storage | Exercise recovery according to the application's RPO/RTO |
| Logs | Diagnostic and audit records | Query historical events and verify retention |

## Operating Rules

- Record backup method, frequency, retention period, and recovery owner for every stateful resource in the asset inventory.
- Put backups and source data in different failure domains. Enable protection against accidental deletion or tampering where needed.
- Check backup job results monthly. At least quarterly, select one critical dataset for a recovery exercise. Record duration, recovery point, missing items, and improvements.
- Before copying production data to Dev/Test, the APP team must approve access and remove sensitive data.

## Current Verification Priorities

Determine the actual storage location and retention period for `weekly-report` input DOCX files, generated PDFs, and downloaded attachments. The current repository does not demonstrate that they are backed up. Verify SharePoint recovery capabilities separately against the actual Microsoft 365 policy.
