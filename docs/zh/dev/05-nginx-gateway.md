# 共享 Nginx 网关、路由与配置发布

Nginx 用作自管服务的统一入口，负责域名路由、TLS 终止、请求转发与访问日志。本页规定以后使用共享网关时的接入和变更方式。

## 接入条件与责任

- 平台负责人管理 Nginx 主机、公共配置、证书部署和变更窗口；应用团队仍负责应用身份验证与授权。
- 每个应用、每个环境使用独立 `server` 配置；Dev/Test 不复用 Prod 域名或后端。
- 网关只转发被批准的路径和端口；内部管理接口不经公网域名暴露。
- 对外应用统一由https转发，不暴露服务端口。

## 配置示例

以下示例用于说明配置结构，域名、证书路径和上游地址均须替换为批准值：

```nginx
server {
    listen 443 ssl;
    server_name app.example.invalid;
    ssl_certificate     /etc/nginx/certs/app.crt;
    ssl_certificate_key /etc/nginx/certs/app.key;
    ssl_protocols TLSv1.2 TLSv1.3;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_connect_timeout 5s;
        proxy_read_timeout 60s;
    }
}
```

## 应用发布

1. 在 GitHub 的网关配置仓库提交 PR；评审域名冲突、上游访问、TLS 和日志设置。
2. 在测试网关环境执行 `nginx -t`，通过后 `systemctl reload nginx`；保留上一版配置和变更记录。
3. 从网关外部访问健康检查与关键页面，检查响应码、证书和访问日志。
4. 失败时恢复上一版配置，再次运行 `nginx -t` 并 reload，通知应用负责人。
5. 正式发布生产网关。

## TODO

网关待建设