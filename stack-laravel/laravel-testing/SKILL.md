---
name: laravel-testing
description: Define testing strategy for Laravel projects with TDD guidance.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Laravel Testing

## Purpose

Provide testing guidance for Laravel using PHPUnit or Pest with unit and feature tests.

## Inputs

* test_framework (string: "phpunit" | "pest")
* scope (list: unit, feature)
* database (string: "sqlite" | "mysql" | "postgres")

## Outputs

* Test folder layout
* Example test patterns
* TDD workflow guidance

## Guidelines

* Use feature tests for HTTP endpoints.
* Use database transactions or refresh for isolation.
* Keep tests deterministic and fast.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: laravel-testing
Inputs:
- test_framework: "pest"
- scope: ["unit", "feature"]
- database: "sqlite"
Output:
- Test layout and examples
- TDD guidance

## Example Invocation

Invoke skill: laravel-testing
Inputs:
- test_framework: "phpunit"
- scope: ["feature"]
- database: "mysql"
Output:
- Feature test examples
- Database isolation notes

## File Structure Example

```
tests/
  Unit/
  Feature/
```
