# Infrastructure：服务器与基础资源

本层记录 Balco 的 Azure 和服务器资源，并提供主机、容器、监控、备份及退役的执行基线。[资源清单与环境拓扑](./01-resource-inventory.md)区分了已核对的 POC 现状和 Dev、Test、Prod 的目标结构。网络、DNS、证书与 TLS 页面目前仅保留标题。

当前已核对范围是 2026-10-12 当前 Azure 登录订阅及本仓库配置。正式生产资产、其他订阅和本地服务器需另行盘点。

## 资源与环境

- [基础资源清单与环境拓扑](/zh/infrastructure/01-resource-inventory)

## 服务器与网络

- [Ubuntu 服务器基线](/zh/infrastructure/04-server-baseline)
- [容器镜像与运行基线](/zh/infrastructure/05-container-runtime)
- [网络、DNS、证书与 TLS](/zh/infrastructure/06-network-and-dns)

## 运行保障

- [存储、备份与保留](/zh/infrastructure/08-storage-and-backup)
- [服务器监控、容量、告警与日志](/zh/infrastructure/10-infrastructure-monitoring)
- [容量规划、成本归属与资源回收](/zh/infrastructure/11-capacity-and-cost)

