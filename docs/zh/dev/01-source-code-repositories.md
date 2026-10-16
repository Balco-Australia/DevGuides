# 代码仓库服务、权限与仓库结构

Balco 的代码、文档和流水线配置统一使用 **GitHub** 管理。新项目优先建立在公司管理的 GitHub Organization 中；仓库所有权、管理员和业务负责人必须能在人员离职后继续交接。

## 仓库划分

- 一个可独立发布的应用或服务使用一个主仓库；共同发布、共同版本管理的前后端可以同仓库。
- 新仓库命名为 `<system>-<component>` or `<system>`，例如 `ForageAI-api`、`ForageAI`。
- 默认建私有仓库。需要公开源代码或文档时，先检查数据、密钥、许可证与发布范围。
- 不在 Git 仓库保存生产数据导出、个人信息、证书私钥、访问令牌或容器镜像二进制。镜像存放在 ACR，详见[制品管理](./02-image-and-artifact-registry.md)。

## 最小目录

```text
.github/workflows/      # CI/CD 流水线
src/                    # 应用或服务源码
tests/                  # 自动化测试
docs/                   # 架构、设计、API、运行手册
README.md               # 用途、本地运行、部署入口
.env.example            # 非敏感配置项示例
```

实际语言或框架可调整目录。

## 角色权限

| 角色 | GitHub 权限 | 责任 |
| --- | --- | --- |
| 平台管理员 | Admin | 仓库设置、规则、紧急恢复 |
| 项目维护者 | Maintain | 评审、版本与发布协调 |
| 开发者 | Write | 分支开发和 Pull Request |
| 审阅者或外部协作者 | Read 或按需授予 | 查看、评审或限定范围协作 |


## TODO

- 创建公司账户，迁移poc Github至公司账户
