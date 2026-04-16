# Infrastructure & DevOps Philosophy (LOCKED) 🔒

> Infrastructure is code or it doesn't exist. Deploys are boring — that's the goal.

## Core Principle

Every piece of infrastructure — servers, databases, DNS, secrets, permissions — is defined in version-controlled config files. If the entire cloud account is destroyed, a single command rebuilds everything. That is the bar.

## Environments

- Local, dev, staging, prod are **identical in structure**. Same OS, same dependencies, same env var schema.
- The only difference between environments is values (credentials, URLs, scale).
- Docker Compose for local. Terraform/Pulumi for cloud. Same source of truth.

## Deploy Pipeline

Every merge to `main` triggers this sequence. No manual steps. No overrides.

```
lint → test → build → deploy staging → smoke tests → deploy prod
```

| Rule | Enforcement |
|---|---|
| Any stage failure stops the pipeline | Merge is reverted or blocked. No partial deploys. |
| Zero-downtime deploys | Rolling or blue-green. The service is never taken offline to ship a change. |
| No `latest` tags | Every container image is pinned to a commit SHA or semantic version. |

## Database Migrations

- Migrations are always **backward-compatible**.
- Add columns — never rename or drop in the same release.
- Two-phase pattern: deploy code that handles both schemas → run migration → deploy cleanup.

## Secrets Management

- No secrets in code. No secrets in committed env files.
- Secrets live in a vault (AWS SSM, HashiCorp Vault, Doppler) and are injected at runtime.
- `grep -r "API_KEY" .` returns only variable references. Anything else is a security incident.

## Containers

- Every service runs in a container. No exceptions.
- Dockerfiles use minimal, multi-stage builds.
- Images are pinned to commit SHA or semver. `latest` is banned.
- If it's not containerized, it's not deployable.