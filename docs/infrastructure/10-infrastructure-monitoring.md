# Server Monitoring, Capacity, Alerts, and Logs

The Infrastructure section monitors resource availability, approaching capacity limits, and the health of backups and certificates. Application request errors, business processes, and user experience measures belong to the APP section; the Dev section maintains the centralized monitoring platform.

## Monitored Signals and Initial Thresholds

| Signal | Warning | Critical | Response |
| --- | --- | --- | --- |
| Self-managed server disk usage | 80% for 15 minutes | 90% for 5 minutes | Clean up, expand, and investigate growth |
| Self-managed server CPU | 80% for 15 minutes | 90% for 15 minutes | Inspect processes, load, and capacity |
| Self-managed server memory | 85% for 15 minutes | 95% for 5 minutes | Investigate memory pressure and restart risk |
| Certificate validity | Fewer than 30 days | Fewer than 7 days | Renew and verify |
