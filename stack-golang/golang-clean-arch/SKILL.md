---
name: golang-clean-arch
description: Apply Clean Architecture boundaries in Go services with clear layering and dependencies.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Go Clean Architecture

## Purpose

Define a Clean Architecture layout for Go services with explicit boundaries between transport, use cases, and data access.

## Inputs

* domain_name (string)
* use_cases (list)
* transports (list)
* persistence (list)

## Outputs

* Layered project structure
* Interfaces and boundary contracts
* Example use case and repository implementation

## Guidelines

* Dependencies flow inward only.
* Use interfaces at boundaries where volatility exists.
* Keep business logic in `internal/usecase` or `internal/core`.
* Keep transport (HTTP/CLI/GRPC) separate from business logic.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: golang-clean-arch
Inputs:
- domain_name: "billing"
- use_cases: ["CreateInvoice", "PayInvoice"]
- transports: ["http"]
- persistence: ["postgres"]
Output:
- Clean Architecture folder layout
- Example use case with repository interface

## Example Invocation

Invoke skill: golang-clean-arch
Inputs:
- domain_name: "billing"
- use_cases: ["CreateInvoice", "PayInvoice"]
- transports: ["http"]
- persistence: ["postgres"]
Output:
- Core entities and interfaces
- Transport adapter skeleton
- Repository implementation

## File Structure Example

```
internal/
  core/
    entity/
    repository/
  usecase/
  transport/
    http/
  data/
    postgres/
```
