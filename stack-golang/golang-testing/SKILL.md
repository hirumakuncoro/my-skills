---
name: golang-testing
description: Provide Go testing guidance with unit, integration, and table-driven tests.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Go Testing

## Purpose

Define testing patterns for Go services using the standard `testing` package and minimal helpers.

## Inputs

* target (string: package or feature)
* test_types (list: unit, integration)
* dependencies (list: db, cache)

## Outputs

* Test file structure
* Sample table-driven tests
* Guidance on mocks and fakes

## Guidelines

* Prefer standard library `testing` and `httptest`.
* Use table-driven tests for inputs/outputs.
* Keep tests deterministic and isolated.
* Use fakes over mocks where possible.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: golang-testing
Inputs:
- target: "internal/service/user"
- test_types: ["unit", "integration"]
- dependencies: ["postgres"]
Output:
- Test layout
- Example tests
- Guidance for test data

## Example Invocation

Invoke skill: golang-testing
Inputs:
- target: "internal/service/user"
- test_types: ["unit"]
- dependencies: []
Output:
- Table-driven tests for service methods
- Suggestions for coverage

## File Structure Example

```
internal/
  service/
    user/
      service.go
      service_test.go
  repository/
    user/
      repo.go
      repo_test.go
```
