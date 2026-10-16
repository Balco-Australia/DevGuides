# Business Central：API 与集成

BC 提供标准 REST API，也支持通过 AL 开发自定义 API。规划集成前，先明确数据归属、同步方向、触发时机和失败后的处理方式，再检查标准 API 是否覆盖需求。

一个简单的集成方案至少要回答：

- 哪个系统是每类数据的权威来源？
- 使用哪个环境、公司和接口版本？
- 重试时如何避免重复创建或重复处理？
- 同步失败时由谁发现、修复和补发？

参考：[REST API 概览](https://learn.microsoft.com/dynamics365/business-central/dev-itpro/webservices/api-overview)、[集成概览](https://learn.microsoft.com/dynamics365/business-central/dev-itpro/developer/integration-overview)。
