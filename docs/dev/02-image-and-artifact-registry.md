# Image and Package Registries and Build Artifacts

Store container images in **Azure Container Registry (ACR)**. The current instance is `balco.azurecr.io` (Central US, Basic). GitHub stores Dockerfiles, build pipelines, and version records. Manage images in three layers: [upstream image → Balco base image → application image](/infrastructure/05-container-runtime).

## Storage Locations

| Content | Location | Rule |
| --- | --- | --- |
| Container base and application images | `balco.azurecr.io` | Record origin, version, scan result, and digest |
| Internal private npm/NuGet/Python packages | GitHub Packages, once enabled | Link to source repositories and grant access by team; do not create a temporary package service beforehand |
| Public dependencies | Official package registries | Use lockfiles and version constraints; do not commit downloaded packages to Git |
| PR build logs and temporary artifacts | GitHub Actions Artifacts | Short-term validation only; retain for 30 days by default; exclude secrets and production data |
| Official release records | GitHub Release or application release log | Link Git commit, artifact digest, and deployment environment |

## Build and Release

Build from a Git commit and record the image `sha256` digest after pushing. Identify images by digest in Test and Prod; do not rely on a mutable `latest` tag. Roll back to the previous validated digest. Before deleting an older image, check whether production or the rollback plan still references it.

Release records must include at least the source commit, Dockerfile and base image versions, image digest, deployment environment, and approval record. The platform owner reviews untagged images, long-unused versions, and ACR storage costs monthly, and informs application owners before cleanup.
