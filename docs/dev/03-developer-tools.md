# Developer Workstations, IDEs, Command-Line Tools, and Local Environments

Developer workstations hold only source code, test data, and temporary build outputs. Do not store production credentials or business data that has not been de-identified. Teams may use managed macOS, Windows 11 with WSL2, or Ubuntu 24.04 LTS; IT asset management assigns the actual devices. This page sets minimum tool and local environment requirements without imposing one language on every project.

## Common Tools

| Tool | Purpose | Version rule |
| --- | --- | --- |
| Git, GitHub CLI | Get code, create PRs, inspect repositories | Use supported versions and sign in with a personal account |
| VS Code or a project-approved IDE | Editing, debugging, and code review | Extensions have traceable sources; the team shares a recommended list |
| Azure CLI | Inspect development resources, debug identity and deployments | Use a personal identity with Dev access only |
| Docker/Podman | Build and run containers locally | Match the target platform architecture and verify images run |
| .NET SDK, Bun/Node, Python | Develop the relevant project | Follow repository manifests and lockfiles, not the machine's latest global version |

## TODO

- Prepare related guides and scripts after the intern completes the development environment setup and configuration.
