---
name: stack-nodejs
description: Node.js and Express backend development guidelines using Prisma and Zod.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Node.js Stack Skill

Guidelines for building backend applications using Node.js, Express, and Prisma.

## Tech Stack
- **Runtime**: Node.js.
- **Framework**: Express.
- **ORM**: Prisma.
- **Validation**: Zod.
- **Testing**: Jest.
- **WebSocket**: Socket.io (Only when requested).

## Design Rules
- **Class-based design only**: Use classes for Services and Controllers.
- Use Zod for schema validation.

## Folder Structure
```
src/
├── controllers/      # Express controllers (Classes)
├── services/         # Business logic (Classes)
├── repositories/     # Data access (Classes)
├── routes/           # Express route definitions
├── middleware/       # Express middlewares
├── utils/            # Utilities
├── app.js            # Express app setup
└── server.js         # Entry point
```

## Coding Rules
- Use `try/catch` or an async wrapper for error handling in controllers.
- Consistent response format for APIs.
- Unit tests for services using Jest.
