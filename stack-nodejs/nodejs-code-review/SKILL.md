---
name: nodejs-code-review
description: Perform a focused code review for Node.js (JavaScript) services.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Node.js Code Review

## Purpose

Review JavaScript Node.js code for correctness, security, performance, and maintainability.

## Inputs

* diff_or_files (string or list)
* context (string: feature or bug)
* test_expectations (string)

## Outputs

* Ordered findings by severity
* Risk assessment and missing tests
* Concrete recommendations

## Guidelines

* Prioritize correctness and regressions.
* Check for async error handling gaps.
* Verify validation and security boundaries.
* Confirm tests cover critical paths.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: nodejs-code-review
Inputs:
- diff_or_files: ["src/controllers/todo.js", "src/routes/todo.js"]
- context: "Add delete endpoint"
- test_expectations: "Add route tests"
Output:
- Review findings
- Risks and test gaps

## Example Invocation

Invoke skill: nodejs-code-review
Inputs:
- diff_or_files: "git diff --stat"
- context: "Refactor auth middleware"
- test_expectations: "No behavior change"
Output:
- Potential regressions
- Missing tests

## File Structure Example

```
src/
  routes/
  controllers/
  middlewares/
  services/
```
