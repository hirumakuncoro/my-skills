---
name: infra-base
description: Infrastructure and deployment guidelines using containerization, orchestration, and devops tooling.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

----------------

# Infrastructure Base

This skill defines baseline infrastructure standards, tooling, and workflows for local development and production deployments.

## Core Tools

* Docker (min 24.x) and Docker Compose v2
* PostgreSQL (min 15.x)
* Redis (min 7.x)
* RabbitMQ (min 3.12.x)
* CI/CD: GitHub Actions
* IaC: Terraform or equivalent (if infrastructure is provisioned in cloud)
* Monitoring: Prometheus + Grafana (or hosted equivalents)

## Workflows

### Local Development

* Provide `Dockerfile` and `docker-compose.yml` for all services.
* Use `.env.example` with documented variables.
* Ensure one-command startup (`docker compose up`).

### Staging/Production

* Separate environment configs for staging and production.
* No hard-coded secrets in repositories.
* Enforce least-privilege access to databases and queues.

### Backups & Migrations

* Use schema migrations (Goose, Flyway, or equivalent).
* Automate backups with retention policies.
* Test restores periodically and document procedures.

## Docker Guidelines

* Use multi-stage builds to reduce image size.
* Pin base image versions.
* Provide health checks.
* Avoid running as root when possible.

## PostgreSQL Guidelines

* Default database is PostgreSQL.
* Use indexed columns for primary query paths.
* Prefer explicit migrations over auto-sync in production.

## Redis Guidelines

* Use Redis for caching and lightweight pub/sub only.
* Namespace keys using service name.
* Set TTLs for cache entries.

## RabbitMQ Guidelines

* Use durable queues for critical workloads.
* Define dead-letter queues for retry policies.
* Document routing keys and exchange types.

## CI/CD Guidelines

* Run lint, tests, and build on every PR.
* Build and scan Docker images.
* Deploy only from protected branches.

## Environment Workflows

* Follow 12-factor configuration principles.
* Use versioned releases and rollback strategies.
* Log to stdout/stderr in structured format.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: infra-base
Inputs:
- services: ["api", "worker"]
- data_stores: ["postgres", "redis", "rabbitmq"]
- environments: ["local", "staging", "production"]
Output:
- Docker and compose files
- CI workflow skeleton
- Environment variable templates

## Example Invocation

Invoke skill: infra-base
Inputs:
- services: ["api", "worker"]
- data_stores: ["postgres", "redis"]
- environments: ["local", "staging", "production"]
Output:
- Dockerfile and docker-compose.yml
- GitHub Actions pipeline steps
- Backup and migration guidance

## File Structure Example

```
infra/
  docker-compose.yml
  Dockerfile
  .env.example
  ci/
    github-actions.yml
```
