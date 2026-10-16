# Business Central：用户与权限

BC 的访问由身份、许可证和应用内权限共同决定。应用内通常通过**权限集**控制用户可访问的对象和操作；在具体项目中还要核对用户所处的公司及业务职责。

在设计自建服务与 BC 的连接时，应分别回答：最终用户能看到什么、集成服务能调用什么、谁有权授予或调整这些权限。不要把自建应用的登录成功等同于拥有 BC 数据权限。

参考：[管理 BC 访问](https://learn.microsoft.com/dynamics365/business-central/admin-access-overview)、[定义权限](https://learn.microsoft.com/dynamics365/business-central/ui-define-granular-permissions)。
