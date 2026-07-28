---
name: stack-bun
description: Bun and TypeScript backend development guidelines using Hono and Prisma.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Bun Stack Skill

Guidelines for building backend applications using Bun, TypeScript, Hono, and Prisma.

## Tech Stack
- **Runtime**: Bun.
- **Language**: TypeScript.
- **Routing**: Hono.
- **ORM**: Prisma.
- **Validation**: Zod.
- **Testing**: Jest/Bun Test.

## Design Rules
- **Class-based design only**: Use classes for Services, Controllers, and Repositories.
- Strict typing with TypeScript.
- Use Zod for request validation.

## Folder Structure
```
src/
├── controllers/      # Request handlers (Classes)
├── services/         # Business logic (Classes)
├── repositories/     # Data access (Classes)
├── models/           # Zod schemas and types
├── middleware/       # Hono middlewares
├── config/           # Configuration
├── index.ts          # Entry point
└── prisma/           # Prisma schema and migrations
```

## Coding Rules
- Prefer `async/await`.
- Use dependency injection (manual) by passing dependencies to class constructors.
- Keep controllers thin; logic belongs in services.
