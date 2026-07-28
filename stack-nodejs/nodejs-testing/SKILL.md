---
name: nodejs-testing
description: Define testing strategy for JavaScript Node.js services.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Node.js Testing

## Purpose

Provide a minimal, effective testing setup for JavaScript Node.js APIs.

## Inputs

* test_runner (string: "node:test")
* http_testing (string: "supertest" or "fetch")
* scope (list: unit, integration)

## Outputs

* Test folder layout
* Sample test patterns
* Guidance on fixtures and cleanup

## Guidelines

* Prefer built-in `node:test` for minimal dependencies.
* Use HTTP integration tests for critical endpoints.
* Keep tests deterministic and isolated.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: nodejs-testing
Inputs:
- test_runner: "node:test"
- http_testing: "supertest"
- scope: ["unit", "integration"]
Output:
- Testing setup
- Example tests

## Example Invocation

Invoke skill: nodejs-testing
Inputs:
- test_runner: "node:test"
- http_testing: "fetch"
- scope: ["integration"]
Output:
- Integration test example
- Guidance on fixtures

## File Structure Example

```
test/
  unit/
  integration/
src/
  app.js
```
