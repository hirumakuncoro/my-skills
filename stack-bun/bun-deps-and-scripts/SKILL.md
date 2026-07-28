---
name: bun-deps-and-scripts
description: Manage Bun dependencies and scripts for TypeScript projects.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Bun Dependencies and Scripts

## Purpose

Define dependency management and common scripts for Bun TypeScript projects.

## Inputs

* dependencies (list)
* scripts (list)
* tooling (list: lint, format, test)

## Outputs

* `package.json` scripts
* Dependency list with reasoning
* Basic tooling guidance

## Guidelines

* Keep dependencies minimal and explicit.
* Prefer Bun built-ins over extra packages.
* Use `bun add` for dependencies and `bun remove` to prune.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: bun-deps-and-scripts
Inputs:
- dependencies: ["zod"]
- scripts: ["dev", "test", "lint"]
- tooling: ["lint", "test"]
Output:
- `package.json` updates
- Script guidance

## Example Invocation

Invoke skill: bun-deps-and-scripts
Inputs:
- dependencies: ["zod"]
- scripts: ["dev", "test"]
- tooling: ["test"]
Output:
- Minimal scripts
- Dependency notes

## File Structure Example

```
package.json
bun.lockb
scripts/
```
