---
name: laravel-ui-options
description: Provide guidance for Laravel UI options including Blade and React/Inertia.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Laravel UI Options

## Purpose

Guide selection and setup of Laravel UI approaches: Blade or React/Inertia.

## Inputs

* ui_stack (string: "blade" | "react-inertia")
* auth (boolean)
* build_tool (string: "vite")

## Outputs

* UI setup guidance
* Folder structure alignment
* Recommended packages

## Guidelines

* Use Blade for server-rendered apps with minimal JS.
* Use React/Inertia for SPA-like experiences without a separate API.
* Keep UI and backend cohesive within Laravel conventions.

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

## Invocation Template

Invoke skill: laravel-ui-options
Inputs:
- ui_stack: "blade"
- auth: true
- build_tool: "vite"
Output:
- Blade UI setup guidance
- Authentication scaffolding notes

## Example Invocation

Invoke skill: laravel-ui-options
Inputs:
- ui_stack: "react-inertia"
- auth: false
- build_tool: "vite"
Output:
- Inertia setup guidance
- React component structure

## File Structure Example

```
resources/
  views/
  js/
    Pages/
    Components/
```
