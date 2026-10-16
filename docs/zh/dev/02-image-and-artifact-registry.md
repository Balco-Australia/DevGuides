# 容器镜像仓库、包仓库与构建制品

容器镜像统一保存在 **Azure Container Registry（ACR）**；当前实例为 `balco.azurecr.io`（Central US，Basic）。GitHub 保存 Dockerfile、构建流水线和版本记录。镜像按[原始镜像 → Balco 基础镜像 → 应用镜像](/zh/infrastructure/05-container-runtime)三层管理。

## 存储位置

| 内容 | 存放位置 | 规则 |
| --- | --- | --- |
| 容器基础镜像、应用镜像 | `balco.azurecr.io` | 记录来源、版本、扫描结果与摘要 |
| 内部私有 npm/NuGet/Python 包 | GitHub Packages（启用后） | 与源码仓库关联，按团队授权；未启用前不自建临时包服务 |
| 公共依赖 | 官方包仓库 | 使用锁文件和版本约束，不把下载包提交 Git |
| PR 构建日志与临时产物 | GitHub Actions Artifacts | 只用于短期验证，默认保留 30 天；不包含密钥或生产数据 |
| 正式发布记录 | GitHub Release 或应用发布记录 | 关联 Git 提交、制品摘要和部署环境 |

## 构建与发布

流水线从 Git 提交构建，推送后记录镜像摘要 `sha256`。测试与生产部署使用摘要定位，不能依赖可变的 `latest`。回滚使用上一个经过验证的摘要；删除旧镜像前检查是否仍被生产或回滚计划引用。

发布记录至少包含：源码提交、Dockerfile 与基础镜像版本、镜像摘要、部署环境和审批记录。平台负责人每月检查无标签镜像、长期未使用版本和 ACR 存储费用；清理前通知应用负责人。
