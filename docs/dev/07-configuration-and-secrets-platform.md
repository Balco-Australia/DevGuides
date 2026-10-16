# Shared Configuration, Secrets, and Credentials

Classify configuration as **public configuration, restricted configuration, or secrets**. Document variable names, defaults, and purposes in the repository. Never put actual secrets in Git, images, documentation, logs, or frontend build outputs.

## Storage Rules

| Type | Examples | Location |
| --- | --- | --- |
| Public configuration | Public API address, Entra tenant/client IDs, feature flags | GitHub Variables, environment configuration files, or Azure App Settings |
| Restricted configuration | Internal endpoints, site IDs, non-public resource names | Access-controlled environment configuration; do not disclose in public repositories |
| Secrets | Client secrets, API keys, connection passwords, certificate private keys | Azure Key Vault; protected secret storage for local use |
