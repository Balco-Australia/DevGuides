# Capacity Planning, Cost Allocation, and Resource Reclamation

## Monthly Review

1. Export the previous month's costs for each resource group and compare them with budget and intended use.
2. Check CPU, memory, request volume, storage, log ingestion, and growth trends.
3. Flag resources without an owner or environment tag, or with no recorded use for 30 consecutive days. The owner decides whether to retain, downsize, or delete them.
4. For planned additions or expansions, record the current constraint, expected growth, monthly cost change, and rollback plan.

## Budgets and Alerts

Assign costs to each production system and shared platform. An initial budget alert can notify owners when the monthly forecast reaches 80% of budget and escalate for review at 100%; this is not an automatic shutdown threshold. Resource owners confirm cost anomalies and unused resources monthly.

## Retirement Process

Confirm dependencies and data retention first, then stop scheduled jobs, revoke access, export data that must be retained, and finally delete resources and DNS/alert configuration. Keep the resource ID, retirement date, executor, and final cost record in the asset inventory.
