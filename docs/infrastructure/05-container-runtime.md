# Container Images and Runtime Baseline

Balco organizes container images in three layers: **upstream image → Balco base image → application image**. Each layer contains only what it owns, making the base environment easier to maintain consistently and application image origins easier to trace.

The current Azure Container Registry (ACR) is `balco.azurecr.io` in Central US, on the Basic SKU. Store image build files and pipeline configuration in GitHub, and push built images to ACR.

## 1. Upstream Images

Upstream images come from official or reviewed upstream publishers; Balco does not modify them directly. When selecting one, record its publisher, version, image digest, purpose, and update date. Pin the digest during builds so newly published content behind the same tag does not enter downstream images without validation.

| Scenario | Example upstream image | Usage |
| --- | --- | --- |
| General Linux runtime | Official `ubuntu:24.04` | Use only when an application needs Ubuntu user space |
| Python task | Official `python:3.12-slim` | The current `weekly_report/Dockerfile` uses this image family |
| .NET 10 container app | Microsoft `mcr.microsoft.com/dotnet/aspnet:10.0`; build stage uses `mcr.microsoft.com/dotnet/sdk:10.0` | Use only if a .NET app is containerized; the current `poc_api` is deployed as code to App Service |

Do not use an upstream image directly as Balco's production application release image. When a high-severity vulnerability is found, a version reaches end of maintenance, or an upstream patch is released, update the Balco base image first and rebuild affected application images.

## 2. Balco Base Images

Balco builds base images on upstream images to provide reusable runtime defaults, such as required CA certificates, time zone settings, security updates, and a non-root runtime user. Include only components needed by multiple applications. Keep application-specific dependencies in application images to avoid oversized or coupled base images.

The supported versions, account permissions, and patch principles in the [Ubuntu server baseline](./04-server-baseline.md) are useful references. Host disk partitions and network subnets do not belong in container images.

Store base images in ACR repositories named `base/<runtime>`, for example:

```text
balco.azurecr.io/base/ubuntu:24.04-balco.1
balco.azurecr.io/base/python:3.12-balco.1
balco.azurecr.io/base/dotnet-aspnet:10.0-balco.1
```

`<runtime>` identifies the runtime, while `balco.<revision>` counts Balco revisions for the same upstream version. For every release, record the upstream image digest, Balco Dockerfile version, scan result, and new image digest. The infrastructure owner maintains base images and asks application teams to rebuild and test after an update.

## 3. Application Images

Build application images from a released Balco base image, adding only application code, dedicated dependencies, and startup configuration. Name new repositories `<system>/<component>` in lowercase with hyphens between words. Do not put the environment in the repository name; deploy the same image through Dev, Test, and Prod.

```text
balco.azurecr.io/weekly-report/worker:v1.0
balco.azurecr.io/procurement/api:v0.3
balco.azurecr.io/procurement/web:v0.3
```

- Use the system short name from the asset inventory for `<system>` and a stable responsibility such as `api`, `web`, or `worker` for `<component>`.
- Keep the current `v<major>.<minor>` version tag form; add a third component for patches, such as `v1.0.1`.
- Do not deploy Test or Prod based on `latest`. Roll back to the previous validated image digest.

## 4. Build, Release, and Runtime Checks

1. Keep Dockerfiles, version records, and build pipelines for all three image layers in GitHub. Application Dockerfiles must explicitly reference a released Balco base image.
2. Scan base images and application dependencies during builds. Record the resulting image digest and push it to ACR. Grant push access only to the build identity and pull access only to runtime identities.
3. After Dev and Test validation, deploy the **same image digest** to Prod; do not rebuild in production.

## TODO

- Create Balco base images for the three layers and pilot them with `weekly-report`.
- Create or designate a GitHub repository for image builds and DevOps, with pipelines to build, scan, and push to `balco.azurecr.io`.
