# CI, Integration Testing, and CD Pipelines

Balco plans three types of pipeline with **GitHub Actions**: CI provides fast feedback and quality gates; the daily integration-test pipeline repeats CI checks, builds a candidate artifact from `dev`, runs integration tests, and publishes a documentation preview; CD handles formal deployments and documentation releases. Branches follow [working branch → dev → main](./09-git-workflow.md), and deployment targets are isolated under [environment management](./06-dev-test-prod-management.md).

| Pipeline | Trigger | Main outputs |
| --- | --- | --- |
| CI | Working-branch PR to `dev`, and changes merged into `dev` | Check results, unit-test report, validation build |
| Integration test | Daily schedule against the latest `dev` commit; manual rerun when needed | Candidate artifact, Test validation report, artifact digest, internal documentation preview |
| CD | Candidate passes tests, `dev → main` PR is merged, and release is approved | Prod deployment record, formal documentation, health checks, rollback target |

## 1. CI: Quality Tools and Gates

CI catches problems early in PRs. Changes can merge into `dev` only after checks pass. Run them again after merge to confirm the integrated branch still builds. CI builds are for validation; the daily integration-test pipeline creates formal candidate artifacts. The daily build runs the same CI checks and adds integration tests and documentation preview publishing, rather than maintaining a separate quality standard.

| Check | Gate rule |
| --- | --- |
| Compilation, type checking, formatting, and static analysis | New errors block merging; record and schedule cleanup of existing issues |
| Unit tests | Required test failures block merging; projects record coverage requirements and changes |
| Dependency and open-source license scans | Fix high-risk dependencies and license conflicts or record a time-limited risk exception |
| Secret scanning | Real credentials block merging immediately; revoke and rotate them |
| Validation build and container scan | Build failures or unresolved critical image vulnerabilities block merging |
| Documentation build and link checks | Build failures or broken internal links block merging |

Record locally runnable check commands in each repository. Make the required checks conditions for merging to `dev` in its GitHub Ruleset. `dev → main` PRs must pass applicable CI checks too. Handle false scan positives and temporarily unfixable issues through the [standards-exception process](./14-team-governance.md).

## 2. Integration Testing: Daily `dev` Build

Each day, the test pipeline checks out the latest `dev` commit and records its full commit SHA. It first runs every required CI check, then builds a releasable candidate artifact and deploys it to Test. Push container apps to ACR and record image digests; retain build packages and checksums for non-container apps.

After checks and tests pass, publish documentation from that `dev` commit to an **internal preview site** for developers and reviewers. If publishing fails, the daily build fails and the version is not marked releasable. The record includes the **commit SHA, candidate artifact digest, test report, documentation preview URL, and validation time**. The test pipeline does not directly publish applications or documentation to Prod; see [documentation as code](./13-documentation-as-code.md) for the rules.

## 3. CD: Deployment and Publication

The release owner opens a `dev → main` PR for a candidate that passed integration testing. Before merging, confirm the `dev` commit in the PR matches the commit SHA in the test report. If new commits reached `dev` after testing, retest the current commit. After review and merge, the CD pipeline verifies that the release code matches the validated commit and the artifact digest matches the test record. The application owner then approves the Prod release.

CD deploys from the recorded ACR image digest or build-package checksum and **does not rebuild application artifacts in Prod**. After deployment, run health checks and key business validations. Once they pass, publish the matching release documentation to the **formal documentation site**. If documentation publishing fails, do not mark the release complete; fix the issue or roll back to the previous documentation version. If application checks fail, stop the release and roll back to the previous validated artifact under [version and artifact traceability](./10-version-and-artifact-policy.md).

## TODO

- Before DevOps work begins, do not build dedicated pipelines; use existing GitHub Actions templates only.
