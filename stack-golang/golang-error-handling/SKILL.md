---
name: golang-error-handling
description: Standardize error handling patterns in Go services.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Go Error Handling

## Purpose

Define consistent error handling and propagation patterns for Go codebases.

## Inputs

* layers (list: http, service, repo)
* error_types (list: validation, not_found, conflict)

## Outputs

* Error type definitions
* Wrapping and sentinel error patterns
* Mapping of internal errors to HTTP responses

## Guidelines

* Use `fmt.Errorf("%w", err)` for wrapping.
* Use sentinel errors for stable classifications.
* Avoid panics in normal control flow.
* Map domain errors to transport-specific responses at boundaries.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: golang-error-handling
Inputs:
- layers: ["http", "service", "repo"]
- error_types: ["validation", "not_found", "conflict"]
Output:
- Error definitions and mapping guidelines
- Example wrapper usage

## Example Invocation

Invoke skill: golang-error-handling
Inputs:
- layers: ["http"]
- error_types: ["validation", "not_found"]
Output:
- Error response mapping
- Validation error patterns

## File Structure Example

```
internal/
  errors/
    types.go
    map_http.go
```
