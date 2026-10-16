# Infrastructure: Servers and Foundational Resources

This section records Balco's Azure and server resources and sets operational baselines for hosts, containers, monitoring, backups, and retirement. The [resource inventory and environment topology](./01-resource-inventory.md) distinguishes the verified POC state from the intended Dev, Test, and Prod structure. The network, DNS, certificates, and TLS page currently contains only a heading.

The verified scope covers the Azure subscription signed in on 12 October 2026 and configuration in this repository. Production assets, other subscriptions, and on-premises servers require a separate inventory.

## Resources and Environments

- [Resource inventory and environment topology](/infrastructure/01-resource-inventory)

## Servers and Network

- [Ubuntu server baseline](/infrastructure/04-server-baseline)
- [Container images and runtime baseline](/infrastructure/05-container-runtime)
- [Network, DNS, certificates, and TLS](/infrastructure/06-network-and-dns)

## Operations

- [Storage, backup, and retention](/infrastructure/08-storage-and-backup)
- [Server monitoring, capacity, alerts, and logs](/infrastructure/10-infrastructure-monitoring)
- [Capacity, cost allocation, and resource reclamation](/infrastructure/11-capacity-and-cost)
