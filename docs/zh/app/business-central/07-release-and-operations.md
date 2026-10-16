# Business Central：发布与运维

BC 相关变更可能包括配置、权限、扩展和外部集成。发布前应在沙盒环境验证业务流程、数据变化及接口兼容性，并记录受影响的公司和外部系统。

运行阶段重点关注扩展安装或升级结果、接口错误、同步积压和用户反馈。BC 可向 Azure Application Insights 发送环境和扩展的遥测数据，具体启用方式以后再结合 Balco 环境确定。

参考：[环境管理](https://learn.microsoft.com/dynamics365/business-central/dev-itpro/administration/tenant-admin-center-environments)、[遥测概览](https://learn.microsoft.com/dynamics365/business-central/dev-itpro/administration/telemetry-overview)。
