---
name: nodejs-error-handling
description: Standardize error handling patterns for Node.js JavaScript APIs.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Node.js Error Handling

## Purpose

Define consistent error handling and response mapping for Node.js APIs.

## Inputs

* error_types (list)
* framework (string: "express")
* response_format (string)

## Outputs

* Error class definitions
* Error middleware
* Mapping rules for HTTP responses

## Guidelines

* Centralize error handling in middleware.
* Normalize error responses (code, message, details).
* Avoid leaking internal errors to clients.
* Handle async errors explicitly.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: nodejs-error-handling
Inputs:
- error_types: ["validation", "not_found", "conflict"]
- framework: "express"
- response_format: "json"
Output:
- Error middleware
- Error class templates

## Example Invocation

Invoke skill: nodejs-error-handling
Inputs:
- error_types: ["validation", "not_found"]
- framework: "express"
- response_format: "json"
Output:
- Standard JSON error responses
- Error mapping rules

## File Structure Example

```
src/
  errors/
    index.js
  middlewares/
    error-handler.js
```
