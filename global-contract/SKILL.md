---
name: global-contract
description: Global engineering principles and shared rules for all development tasks. Defines the core philosophy, design preferences, and code quality enforcement.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

----------------

# Global Engineering Contract

This contract is mandatory for all skill executions. It defines baseline principles, design preferences, output rules, and strict prohibitions that all generated work must obey.

## Core Principles

* KISS
* DRY
* Readability > Cleverness
* Explicit > Magic
* Maintainability First

## Design Preferences

* Prefer simple, composable modules over large abstractions.
* Prefer standard libraries and built-ins over third-party packages.
* Favor clear, explicit interfaces and predictable data flow.
* Keep configuration explicit and environment-driven.
* Prefer deterministic behavior and reproducible builds.

## Output Rules

* Show a folder structure before file content when generating new projects.
* Keep functions short and cohesive.
* Avoid unused dependencies and dead code.
* Add comments only when logic is non-obvious.
* Include basic tests for critical paths.
* Prefer explicit errors and validation near input boundaries.

## Code Quality Enforcement

* Must be runnable with documented steps.
* Must be testable and include at least a minimal test suite.
* Must separate concerns (transport, business logic, data access).
* Must be dependency-minimal.
* Must avoid global mutable state.

## Strict Prohibitions

* No overengineered patterns or premature abstractions.
* No DDD-heavy modeling unless explicitly requested.
* No generic repositories as default.
* No reflection or metaprogramming unless explicitly requested.
* No dependency injection frameworks unless explicitly requested.
* No complex builder patterns for simple objects.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Templates

Invoke skill: <skill-name>
Inputs:
- <key>: <value>
Output:
- <expected deliverables>

## Example Invocations

Invoke skill: golang-http-server
Inputs:
- service_name: "accounts"
- router: "net/http"
- storage: "postgres"
Output:
- Folder structure
- HTTP handlers, routes, and config
- Basic tests
Constraints:
- Apply global-contract principles

Invoke skill: nodejs-create-api
Inputs:
- resource: "Todo"
- fields:
  - id: number
  - name: string
  - done: boolean
Output:
- Express API skeleton
- Tests for CRUD
- Folder structure
Constraints:
- Apply global-contract principles

Invoke skill: bun-typescript-api
Inputs:
- service_name: "billing"
- transport: "Bun.serve"
- db: "postgres"
Output:
- Typed API skeleton
- Request validation
- Basic tests
Constraints:
- Apply global-contract principles
