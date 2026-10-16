# Ubuntu Server Baseline

This standard applies to physical and virtual machines whose operating systems Balco manages. Azure manages the host operating systems of App Service, Static Web Apps, and Container Apps, so the host installation instructions here do not apply to them. The current subscription lists no Azure VMs. The two physical machines in the [resource inventory](./01-resource-inventory.md) are sample data and do not indicate deployment under this standard.

## 1. Operating System and Installation

- Install **64-bit Ubuntu Server 24.04 LTS** on new hosts, using official installation media or a verified internal image. Record compatibility reasons and an upgrade plan for any version exception.
- Select minimal components, UEFI boot, and a GPT partition table. Name hosts `balco-<env>-<role>-<nn>`, for example `balco-dev-build-01`.
- After installation, record the host serial number, Ubuntu version, kernel, CPU, memory, disks, MAC address, management IP, environment, and owner. Enable time synchronization and security updates.
- Before handing over a new host, check patches, accounts, SSH, disks, network, firewall, monitoring, and backups.

The [official Ubuntu maintenance schedule](https://ubuntu.com/security/esm) shows that 24.04 LTS remains in standard support. Validate application compatibility in Test before upgrading to a newer LTS release, then update this baseline.

## 2. Users and Permissions

| Account | Purpose | Permissions |
| --- | --- | --- |
| `balcoadmin` | Platform administrator for installation, patching, and incident handling | Can use `sudo`; does not run routine business processes |
| `balco01` | Daily sign-in account for the first developer or operator | Standard user; no `sudo` by default |
| `balco02` | Daily sign-in account for the second developer or operator | Standard user; no `sudo` by default |
| `balco03`… | Subsequent people receive sequential numbers | Standard user; permissions requested by role |
| Application service account | Runs one service or scheduled task | No interactive sign-in; access limited to its own directories and data |

- Assign each `balcoNN` account to **one person only**. Record the person, request, and deactivation date in the asset register. Do not share accounts or immediately reassign a departed person's number.
- Platform administrators manage the `balcoadmin` key and retain an operation record whenever it is used. Prefer personal `balcoNN` accounts for daily work. When elevation is necessary, approve it and add the account to the `sudo` group for a limited time.
- Disable SSH password authentication and direct root sign-in; use personal public keys. Revoke the account, SSH key, and temporary elevation when a person leaves or changes roles.
- Run system services under separate service accounts, never under `balcoadmin` or a person's account.

## 3. Disks and Partitions

A new host should have a **system disk of at least 100 GiB**, using GPT + LVM + ext4. Adjust capacity to the workload, but separate the system, logs, and business data.

| Mount point | Initial size | Purpose and rules |
| --- | --- | --- |
| EFI System Partition | 1 GiB | UEFI boot |
| `/boot` | 2 GiB | Kernel and boot files |
| `/` | 40 GiB | Operating system and system software |
| `/var` | 20 GiB | Software state and cache; no unbounded application data |
| `/var/log` | 10 GiB | System logs, with rotation and capacity alerts |
| swap | 4 GiB | Buffer for abnormal memory pressure, not a substitute for adding RAM |
| Unallocated LVM space | Remainder | Reserved for system expansion |

Mount business data from a separate data disk at `/srv/<service>`. If running Docker, put `/var/lib/docker` on a separate data volume so images and container logs cannot fill the root partition. Choose encryption, RAID, and the file system according to data sensitivity and recovery objectives. At handover, record disk serial numbers, capacity, mount points, file systems, and backup methods.

## 4. Network Segmentation

The following CIDRs are **planning examples for new environments**. Check them against the existing corporate network and Azure VNet before implementation to avoid address conflicts.

| Network zone | Example subnet | Resources | Default access rule |
| --- | --- | --- | --- |
| Management | `10.20.10.0/24` | Jump host and operations entry point | Only administrators reach host SSH through VPN or jump host |
| Dev | `10.20.20.0/24` | Development hosts and services | Approved development dependencies only; no direct Prod data access |
| Test | `10.20.30.0/24` | Test hosts and services | Test data and approved shared services only |
| Prod application | `10.20.40.0/24` | Production application hosts | Gateway and necessary downstream ports only |
| Data | `10.20.50.0/24` | Databases and storage | Designated application zones and backup services only |

Give each host a fixed management address or DHCP reservation and record its DNS, gateway, and firewall rules. Do not expose SSH port 22 to the internet. Deny Dev/Test access to Prod by default. Route public application entry points through a controlled gateway where possible instead of exposing host ports directly. The shared network, DNS, and certificate standard will be written on the [Network, DNS, Certificates, and TLS](./06-network-and-dns.md) page.

## 5. Maintenance and Acceptance

Assess routine patches monthly and schedule a maintenance window. Aim to remediate or isolate high-severity vulnerabilities that are exploited or affect public services within seven days. Monitor host CPU, memory, disks, network, and time synchronization. Collect logs centrally and back up systems and data under the [backup standard](./08-storage-and-backup.md).

Handover acceptance must cover at least: correct Ubuntu version; registered accounts and SSH keys; disabled root and password sign-in; partitions and mounts matching the design; reviewed network rules; one actual monitoring and backup trigger; and an identified recovery owner. Record the reason, owner, compensating controls, and expiry date for any deviation from the baseline.
