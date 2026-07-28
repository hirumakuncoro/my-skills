---
name: nodejs-create-api
description: Create a JavaScript Node.js REST API with minimal dependencies and clear structure.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Node.js Create API

## Purpose

Generate a JavaScript (non-TypeScript) REST API skeleton with routing, validation, and basic tests.

## Inputs

* service_name (string)
* framework (string: "express")
* resources (list)
* data_store (string: "memory" | "postgres" | "mysql")

## Outputs

* API server scaffold
* Route handlers and controllers
* Validation and error middleware
* Basic tests

## Guidelines

* Use JavaScript (CommonJS or ESM, but be consistent).
* Keep dependencies minimal.
* Separate routes, controllers, and services.
* Validate inputs at request boundaries.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: nodejs-create-api
Inputs:
- service_name: "todo-api"
- framework: "express"
- resources:
  - name: "Todo"
    fields: ["id", "name", "done"]
- data_store: "memory"
Output:
- Express API skeleton
- CRUD handlers
- Tests for routes

## Example Invocation

Invoke skill: nodejs-create-api
Inputs:
- service_name: "todo-api"
- framework: "express"
- resources:
  - name: "Todo"
    fields: ["id", "name", "done"]
- data_store: "postgres"
Output:
- Express routes and controllers
- Repository layer
- Basic route tests

## File Structure Example

```
src/
  routes/
  controllers/
  services/
  repositories/
  middlewares/
  app.js
  server.js
test/
```
