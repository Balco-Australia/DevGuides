# Centralized Logs, Metrics, Traces, and Alerts

The Dev section provides a shared observability platform. Azure workloads should use **Azure Monitor, Application Insights, and Log Analytics** where possible. Collect self-managed Nginx and Ubuntu host data centrally for the same search and alert system. The APP section defines business metrics and application alerts; Infrastructure owns host and resource availability.

## Logging Standard

Each application log entry should include at least a UTC timestamp, environment, system, service, version, severity, message, and correlation ID. HTTP requests also record method, route, status code, and duration. Use structured fields so one request can be traced through the gateway, API, and downstream calls. Keep Nginx access and error logs, and collect system and security events from Ubuntu hosts.

Do not log passwords, access tokens, full personal information, or sensitive business content. Use controlled user identifiers or masked fields when user identification is needed for diagnosis. Grant log-reading access by role and audit access to production logs.

## Metrics and Alerts

| Resource | Basic signals | Default responder |
| --- | --- | --- |
| Nginx gateway | Availability, 5xx responses, upstream connection failures, certificate expiry | Platform owner |
| API / Web | Request volume, error rate, latency, critical dependency failures | Application owner |
| Container Apps Job | Success rate, failures, execution duration | Job owner |
| Azure/Ubuntu resources | CPU, memory, disk, backup results | Infrastructure owner |

For each alert, record the threshold, severity, notification group, on-call person, runbook, and closure condition. Prioritize **service outages, sustained errors, job failures, backup failures, and expiring certificates** in the first version. Trigger test events before enabling alerts to confirm delivery. Review unowned, duplicate, and long-silent alert rules monthly.

## Retention and Validation

Retain Dev logs for 14 days, Test logs for 30 days, and Prod operational logs for 90 days by default. Set audit-log retention separately according to data classification and compliance needs. These are initial targets; check Azure costs and current workspace policies before enabling them. After a release, use one traceable request to validate logs, metrics, and correlation IDs, then test the notification path with a sample alert.

## TODO

- Validate in a real project.
- Establish a shared logging standard.
