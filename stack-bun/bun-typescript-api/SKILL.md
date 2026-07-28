---
name: bun-typescript-api
description: Build a TypeScript API using Bun runtime with minimal dependencies.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Bun TypeScript API

## Purpose

Generate a TypeScript API using Bun's runtime and built-in server capabilities.

## Inputs

* service_name (string)
* transport (string: "Bun.serve")
* endpoints (list)
* data_store (string)

## Outputs

* API server bootstrap
* Route handlers
* Request validation skeleton
* Basic tests

## Guidelines

* Use TypeScript exclusively.
* Prefer `Bun.serve` and standard library features.
* Keep routing and handlers simple.
* Validate inputs at boundaries.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: bun-typescript-api
Inputs:
- service_name: "catalog"
- transport: "Bun.serve"
- endpoints:
  - method: "GET"
    path: "/health"
  - method: "POST"
    path: "/items"
- data_store: "postgres"
Output:
- TypeScript API skeleton
- Handlers and routing
- Basic tests

## Example Invocation

Invoke skill: bun-typescript-api
Inputs:
- service_name: "catalog"
- transport: "Bun.serve"
- endpoints:
  - method: "GET"
    path: "/health"
- data_store: "memory"
Output:
- `src/server.ts` with Bun.serve
- Health handler and test

## File Structure Example

```
src/
  server.ts
  routes/
  handlers/
  services/
  config/
test/
```
