---
name: golang-http-server
description: Build a Go HTTP server with routing, middleware, configuration, and graceful shutdown.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Go HTTP Server

## Purpose

Generate a minimal, production-ready HTTP server using Go standard library (or a minimal router if required).

## Inputs

* service_name (string)
* router (string: "net/http" or "chi")
* endpoints (list)
* config (list of env vars)

## Outputs

* HTTP server bootstrap
* Route definitions and handlers
* Configuration loader
* Health endpoints
* Minimal tests

## Guidelines

* Prefer `net/http` unless routing needs justify a router.
* Include graceful shutdown and timeouts.
* Validate input at handler boundaries.
* Keep handlers thin and delegate to service layer.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: golang-http-server
Inputs:
- service_name: "accounts"
- router: "net/http"
- endpoints:
  - method: "GET"
    path: "/health"
  - method: "POST"
    path: "/accounts"
- config: ["PORT", "LOG_LEVEL"]
Output:
- HTTP server skeleton
- Routes and handlers
- Basic tests

## Example Invocation

Invoke skill: golang-http-server
Inputs:
- service_name: "accounts"
- router: "chi"
- endpoints:
  - method: "GET"
    path: "/health"
  - method: "POST"
    path: "/accounts"
- config: ["PORT", "DATABASE_URL"]
Output:
- Server setup and router
- Handler skeletons
- Tests for handlers

## File Structure Example

```
internal/
  server/
    http.go
    routes.go
    handlers/
  service/
  config/
cmd/
  accounts/
    main.go
```
