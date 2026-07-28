---
name: skills-generator
description: Generate detailed SKILL.md files for each skill under `.general/skills`, obeying strict engineering principles like KISS and DRY.
license: MIT
metadata:
  author: Ikhsan Heriyawan
user-invokable: false
---

## 1) GLOBAL CONTRACT SKILL
Generate a detailed `global-contract/SKILL.md` that:
- Enumerates core engineering principles
- Includes constraints to always follow in code generation
- Provides example invocation patterns for other skills
- Enforces output rules and strict prohibitions
Use the following outline:

```

name: global-contract
description: Global engineering principles and shared rules for all development tasks. Defines the core philosophy, design preferences, and code quality enforcement.
license: MIT
metadata:
author: Ikhsan Heriyawan
version: "1.0.0"
----------------

# Global Engineering Contract

## Core Principles

* KISS
* DRY
* Readability > Cleverness
* Explicit > Magic
* Maintainability First

## Design Preferences

...

## Output Rules

...

## Code Quality Enforcement

...

```

Add example invocations that reference applying this contract in code generators.

---

## 2) INFRA BASE SKILL
Generate a detailed `infra-base/SKILL.md` that:
- Describes infrastructure capability
- Lists official tooling, versions, and standards
- Has specific guidelines for Docker, PostgreSQL, Redis, RabbitMQ
- Includes CI/CD, monitoring, testing and environment workflows
- Contains constraints about infrastructure IaC, reproducibility, idempotency

Use this outline:

```

name: infra-base
description: Infrastructure and deployment guidelines using containerization, orchestration, and devops tooling.
license: MIT
metadata:
author: Ikhsan Heriyawan
version: "1.0.0"
----------------

## Core Tools

* Docker & Docker Compose
* PostgreSQL
* Redis, RabbitMQ
* CI/CD (GitHub Actions)

## Workflows

* Local development
* Staging/Production
* Backups & Migrations
  ...

## Constraints

* Always reproducible
* Always documented
* Follow security best practices
  ...

```

---

## 3) STACK SKILLS

For each stack below, create a skill folder with multiple SKILL.md files for specific tasks:

### 3a) GO STACK — under `stack-golang/`
Generate:
- `golang-setup/SKILL.md`
- `golang-http-server/SKILL.md`
- `golang-clean-arch/SKILL.md`
- `golang-testing/SKILL.md`
- `golang-error-handling/SKILL.md`

Each SKILL.md must include:
- Description of purpose
- Inputs & outputs
- Constraints referencing global principles
- Example invocation templates
- Code structure guidelines

---

### 3b) NODEJS STACK — under `stack-nodejs/`
Generate:
- `nodejs-create-api/SKILL.md`
- `nodejs-code-review/SKILL.md`
- `nodejs-error-handling/SKILL.md`
- `nodejs-testing/SKILL.md`

Nodejs referencing JavaScript (not TypeScript).

---

### 3c) BUN STACK — under `stack-bun/`
Generate:
- `bun-init-project/SKILL.md`
- `bun-typescript-api/SKILL.md`
- `bun-deps-and-scripts/SKILL.md`
- `bun-testing/SKILL.md`

Bun stack uses **TypeScript exclusively**.

---

### 3d) LARAVEL STACK — under `stack-laravel/`
Generate:
- `laravel-init-project/SKILL.md`
- `laravel-ui-options/SKILL.md`
- `laravel-api-resources/SKILL.md`
- `laravel-testing/SKILL.md`

Provide guidelines for:
- Blade.php UI
- React/Inertia UI
- API and backend cohesion
- Testing & TDD

---

## 4) PRESERVE EXISTING SKILLS

Do **not rewrite** the following:
- `react-best-practices/SKILL.md`
- `web-design-guidelines/SKILL.md`

Unless there is an explicit missing pattern — in that case generate a diff summary and propose changes only.

---

## 5) GENERAL RULES FOR ALL GENERATED SKILL FILES

All SKILL.md files must include:

```

### Constraints

* Always apply global engineering principles (KISS, DRY, Maintainability, etc.)
* Prefer minimal dependencies
* Include examples
* Provide invocation template
* Provide file structure examples

```

---

## 6) SAMPLE INVOCATION FOR EACH SKILL GENERATOR

Example:
```

Invoke skill: nodejs-create-api
Inputs:
resource: "Todo"
fields:
- id: number
- name: string
- done: boolean
Output:

* Code skeleton
* Suggest tests
* Suggest folder structure

```

---
