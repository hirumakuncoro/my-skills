---
name: stack-golang
description: Golang backend development guidelines using Clean Architecture and Chi router.
license: MIT
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Golang Stack Skill

Guidelines for building backend applications using Go, Chi, and Gorm.

## Tech Stack
- **Routing**: Chi router.
- **ORM**: Gorm.
- **Testing**: testify/assert.
- **Architecture**: Clean Architecture.

## Folder Structure (MANDATORY)
```
.
├── cmd/
│   └── api/
│       └── main.go           # Entry point (setup router & DB)
├── internal/                 # Private code
│   ├── handler/              # HTTP Handlers / Controllers
│   ├── service/              # Business Logic (Usecase)
│   ├── repository/           # Data Access (SQL, Redis, etc.)
│   ├── domain/               # Struct/Entity & Interface Definitions
│   └── middleware/           # Chi Middlewares
├── pkg/                      # Public utility code (Optional)
├── go.mod
└── go.sum
```

## Architecture Rules
- **Handler**: Responsible for parsing requests and sending responses.
- **Service**: Contains business logic; depends on domain interfaces.
- **Repository**: Handles data persistence; implements domain interfaces.
- **Domain**: Contains core entities and interface definitions to avoid circular dependencies.

## Coding Rules
- Follow standard Go idioms.
- Use interfaces to decouple layers.
- Avoid global variables (especially for DB connections).
- Use `testify/assert` for all tests.
- Ensure all business logic in `service` is unit-tested.

## API Requirements
- Every API must have an example request/response documentation or a `.http` file.
- Error handling must be consistent across the application.
