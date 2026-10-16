# Shared Nginx Gateway, Routing, and Configuration Releases

Nginx provides a common entry point for self-managed services, handling domain routing, TLS termination, request forwarding, and access logs. This page defines onboarding and changes for a future shared gateway.

## Entry Conditions and Responsibilities

- The platform owner manages the Nginx host, shared configuration, certificate deployment, and change window. Application teams remain responsible for application authentication and authorization.
- Use a separate `server` configuration for each application and environment. Dev/Test must not reuse Prod domains or backends.
- Forward only approved paths and ports. Do not expose internal management APIs through public domains.
- Route public applications over HTTPS; do not expose service ports directly.

## Configuration Example

This example illustrates the configuration structure. Replace the domain, certificate paths, and upstream address with approved values:

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

## Application Release

1. Submit a PR to the GitHub gateway-configuration repository. Review domain conflicts, upstream access, TLS, and log settings.
2. Run `nginx -t` in the test gateway environment, then `systemctl reload nginx` after it passes. Keep the previous configuration and change record.
3. Access the health check and key pages from outside the gateway. Check response codes, certificates, and access logs.
4. On failure, restore the previous configuration, run `nginx -t` again, reload, and inform the application owner.
5. Release the production gateway configuration.

## TODO

- Build the gateway.
