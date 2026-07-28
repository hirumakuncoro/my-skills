---
name: bun-init-project
description: Initialize a Bun project using TypeScript with standard structure and scripts.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Bun Init Project

## Purpose

Create a Bun project configured for TypeScript with a clean folder structure and standard scripts.

## Inputs

* project_name (string)
* tsconfig (string: "strict" or "default")
* package_type (string: "module")

## Outputs

* `package.json` and `tsconfig.json`
* Standard scripts for dev, test, lint
* Basic project layout

## Guidelines

* Use TypeScript exclusively.
* Keep scripts minimal and explicit.
* Provide an example entry file.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: bun-init-project
Inputs:
- project_name: "inventory"
- tsconfig: "strict"
- package_type: "module"
Output:
- Bun + TypeScript scaffold
- Base scripts

## Example Invocation

Invoke skill: bun-init-project
Inputs:
- project_name: "inventory"
- tsconfig: "strict"
- package_type: "module"
Output:
- `package.json` and `tsconfig.json`
- `src/index.ts`

## File Structure Example

```
src/
  index.ts
package.json
tsconfig.json
bun.lockb
```
