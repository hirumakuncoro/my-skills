---
name: laravel-init-project
description: Initialize a Laravel project with standard configuration and environment setup.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Laravel Init Project

## Purpose

Create a Laravel project with consistent environment configuration, dependencies, and basic tooling.

## Inputs

* project_name (string)
* php_version (string)
* database (string: "mysql" | "postgres")
* use_sail (boolean)

## Outputs

* Laravel project scaffold
* Environment configuration
* Basic database connection setup

## Guidelines

* Use Laravel defaults unless explicit changes are requested.
* Keep environment variables documented in `.env.example`.
* Prefer framework conventions over custom structure.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: laravel-init-project
Inputs:
- project_name: "storefront"
- php_version: "8.2"
- database: "postgres"
- use_sail: true
Output:
- Laravel scaffold
- `.env.example` and base config

## Example Invocation

Invoke skill: laravel-init-project
Inputs:
- project_name: "storefront"
- php_version: "8.2"
- database: "mysql"
- use_sail: false
Output:
- Base Laravel app
- Database config notes

## File Structure Example

```
app/
bootstrap/
config/
database/
resources/
routes/
```
