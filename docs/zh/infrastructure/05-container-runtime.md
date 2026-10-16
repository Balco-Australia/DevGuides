# 容器镜像与运行基线

Balco 的容器镜像分为 **原始镜像 → Balco 基础镜像 → 应用镜像** 三层。每层只加入本层负责的内容，便于统一维护基础环境，并追溯应用镜像的来源。

当前 Azure Container Registry（ACR）为 `balco.azurecr.io`，位于 Central US，SKU 为 Basic。镜像构建文件和流水线配置应保存在 GitHub；构建完成的镜像推送到 ACR。

## 1. 原始镜像

原始镜像来自官方或经过审核的上游发布源，Balco 不直接修改其内容。选型时记录发布方、版本、镜像摘要、用途和更新日期；构建时固定摘要，避免同一标签指向的新内容未经验证就进入下游镜像。

| 场景 | 原始镜像示例 | 使用说明 |
| --- | --- | --- |
| 通用 Linux 运行环境 | 官方 `ubuntu:24.04` | 仅在应用确需 Ubuntu 用户空间时使用 |
| Python 任务 | 官方 `python:3.12-slim` | 当前 `weekly_report/Dockerfile` 使用该系列镜像 |
| .NET 10 容器应用 | Microsoft `mcr.microsoft.com/dotnet/aspnet:10.0`；构建阶段使用 `mcr.microsoft.com/dotnet/sdk:10.0` | 仅在 .NET 应用采用容器部署时使用；当前 `poc_api` 是 App Service 代码部署 |

原始镜像不直接作为 Balco 应用的生产发布镜像。发现高危漏洞、版本停止维护或上游发布新补丁时，先更新 Balco 基础镜像，再重新构建受影响的应用镜像。

## 2. Balco 基础镜像

基础镜像由 Balco 在原始镜像上构建，统一提供可复用的运行基线，例如必要的 CA 证书、时区设置、安全更新和非 root 运行用户。只加入多个应用都需要的组件；应用专属依赖留在应用镜像中，避免基础镜像过大或相互牵连。

[Ubuntu 服务器基线](./04-server-baseline.md)中的受支持版本、账户权限和补丁管理原则可作为参考；磁盘分区、主机网段等主机配置不写入容器镜像。

基础镜像存放在 ACR 的 `base/<runtime>` 仓库中，例如：

```text
balco.azurecr.io/base/ubuntu:24.04-balco.1
balco.azurecr.io/base/python:3.12-balco.1
balco.azurecr.io/base/dotnet-aspnet:10.0-balco.1
```

其中 `<runtime>` 表示运行时，`balco.<revision>` 表示 Balco 对同一上游版本的修订次数。每次发布记录上游镜像摘要、Balco Dockerfile 版本、扫描结果和新镜像摘要。基础镜像由基础设施负责人维护；更新后通知使用它的应用团队重新构建和测试。

## 3. 应用镜像

应用镜像必须从已发布的 Balco 基础镜像构建，只加入应用代码、专属依赖和启动配置。新仓库命名为 `<system>/<component>`：全部小写，单词用连字符；环境不写进仓库名，同一镜像可依次部署到 Dev、Test 和 Prod。

```text
balco.azurecr.io/weekly-report/worker:v1.0
balco.azurecr.io/procurement/api:v0.3
balco.azurecr.io/procurement/web:v0.3
```

- `<system>` 使用资产清单中的系统短名；`<component>` 使用 `api`、`web`、`worker` 等稳定职责名。
- 版本标签沿用项目当前的 `v<主版本>.<次版本>` 形式；需要补丁版本时增加第三段，例如 `v1.0.1`。
- `latest` 不作为 Test 或 Prod 的部署依据；回滚到上一已验证的镜像摘要。


## 4. 构建、发布与运行检查

1. 在 GitHub 仓库保存三层镜像的 Dockerfile、版本记录和构建流水线；应用 Dockerfile 明确引用已发布的 Balco 基础镜像。
2. 构建时扫描基础镜像和应用依赖，生成镜像后记录摘要并推送 ACR。推送权限仅授予构建身份；运行身份仅获得拉取权限。
3. Dev 和 Test 验证通过后，将**同一镜像摘要**用于 Prod，不在生产环境重新构建。

## TODO

- 按三层分类建立 Balco 基础镜像，并先让 `weekly-report` 试用。
- 在 GitHub 建立或指定镜像构建与 DevOps 仓库，配置构建、扫描和推送到 `balco.azurecr.io` 的流水线。
