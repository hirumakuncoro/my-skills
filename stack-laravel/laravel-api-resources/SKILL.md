---
name: laravel-api-resources
description: Define API resources, controllers, and request validation in Laravel.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Laravel API Resources

## Purpose

Provide guidance for building API endpoints using controllers, resources, and request validation.

## Inputs

* resource_name (string)
* fields (list)
* auth (boolean)

## Outputs

* Controller and resource definitions
* Request validation rules
* Route setup guidance

## Guidelines

* Use Form Request validation.
* Use API Resources for consistent response shapes.
* Keep controllers thin; move logic to services when needed.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: laravel-api-resources
Inputs:
- resource_name: "Order"
- fields: ["id", "total", "status"]
- auth: true
Output:
- Controller, Resource, and Request classes
- Route guidance

## Example Invocation

Invoke skill: laravel-api-resources
Inputs:
- resource_name: "Order"
- fields: ["id", "total", "status"]
- auth: false
Output:
- API resource setup
- Validation rules

## File Structure Example

```
app/
  Http/
    Controllers/
    Requests/
    Resources/
routes/
  api.php
```
