# 基础资源清单与环境拓扑

本文记录已核对的资源现状，并规定 Dev、Test、Prod 环境应如何登记和隔离。**现状范围**为 2026-10-12 当前 Azure 登录订阅和仓库配置；它不等于 Balco 全部服务器或正式生产资产清单。

## Azure 资源

资源组 `balco-poc`

| 资源 | Azure 类型 | 用途 | 负责人 |
| --- | --- | --- | --- |
| `balco-poc` | Azure AI Services 及项目 | AI 相关 POC 资源 | Su |
| `balco-poc-web` | Static Web Apps | POC 前端；默认域名 `mango-grass-04a587310.2.azurestaticapps.net` | Su |
| `balco-poc-api` | App Service | POC .NET API；当前状态 Running，`httpsOnly=true` | Su |
| `balco` | Azure Container Registry | 镜像注册表；使用规则见 Dev 层 | Su |
| `weekly-report` | Container Apps Job | 周报任务 | Su |
| `balco-translator` | Azure AI 服务 | 翻译服务资源 | Su |


## 物理机和虚拟机资源

| 主机名 | IP | 环境 | 用户名 | 使用人 |
| --- | --- | --- | --- | --- |
| `dev-01` | xxx | Dev  | `balcoadmin` | Jason, Su |
| `dev-01` | xxx | Dev  | `balco01` | Su |
| `dev-01` | xxx | Dev  | `balco02` |
| `dev-01` | xxx | Dev  | `balco03` |
| `test-01` | xxx | Test  | `balcoadmin` | Jason, Su | 
| `test-01` | xxx | Test  | `balco01`| Su |
| `test-01` | xxx | Test  | `balco02`|
| `test-01` | xxx | Test  | `balco03`|


## 资产登记规则

每项资源登记 `资源组、类型、资源、环境、系统、负责人`。资源创建或变更时同步更新；每月核对一次 Azure 导出清单。非 Azure 服务器另附主机信息。

## TODO

- 从个人账户迁移至公司账户后Azure更新状态
- 物理机虚拟机资源就绪后更新
