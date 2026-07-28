---
name: bun-testing
description: Define testing practices for Bun TypeScript projects.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Bun Testing

## Purpose

Provide a testing strategy using Bun's built-in test runner.

## Inputs

* scope (list: unit, integration)
* http_testing (string: "fetch")

## Outputs

* Test layout
* Example test files
* Guidance for fixtures and cleanup

## Guidelines

* Use `bun test` as the default runner.
* Prefer fast, deterministic tests.
* Keep tests close to the code when possible.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: bun-testing
Inputs:
- scope: ["unit", "integration"]
- http_testing: "fetch"
Output:
- Test layout and examples

## Example Invocation

Invoke skill: bun-testing
Inputs:
- scope: ["unit"]
- http_testing: "fetch"
Output:
- Unit test samples
- Guidance on fixtures

## File Structure Example

```
src/
  handlers/
  handlers.test.ts
test/
  integration/
```
