---
name: golang-setup
description: Initialize a Go service repository with standard structure, tooling, and baseline configuration.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Go Setup

## Purpose

Create a clean, minimal Go service skeleton with consistent tooling and a clear project layout.

## Inputs

* module_name (string)
* go_version (string)
* service_name (string)
* tooling (list: lint, format, test)

## Outputs

* Project folder structure
* `go.mod` and `go.sum`
* Basic configuration for lint, formatting, and tests
* Minimal README with run instructions

## Guidelines

* Use Go modules.
* Keep the root clean; place app code under `cmd/` and `internal/`.
* Prefer standard library over external dependencies.
* Provide a small `Makefile` or scripts for common tasks.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: golang-setup
Inputs:
- module_name: "github.com/acme/payments"
- go_version: "1.22"
- service_name: "payments"
- tooling: ["lint", "test"]
Output:
- Repository skeleton
- Tooling configuration

## Example Invocation

Invoke skill: golang-setup
Inputs:
- module_name: "github.com/acme/payments"
- go_version: "1.22"
- service_name: "payments"
- tooling: ["lint", "test"]
Output:
- Go module and folder structure
- Basic Makefile targets
- README instructions

## File Structure Example

```
.
  cmd/
    payments/
      main.go
  internal/
    config/
    server/
  pkg/
  scripts/
  Makefile
  go.mod
  go.sum
  README.md
```
