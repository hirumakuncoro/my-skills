---
name: stack-laravel
description: Laravel backend development guidelines using PostgreSQL, Inertia, and React.
license: MIT
metadata:
  author: Ikhsan Heriyawan (or you)
  version: "1.0.0"
---

# Laravel Stack Skill

Guidelines for building backend applications using Laravel (PHP) with modern tooling and best practices.

## Tech Stack
- **Framework**: Laravel (latest LTS recommended).
- **Frontend**: React (with Inertia.js).
- **Starter Kit**: Laravel Breeze or Jetstream with Inertia + React stack.
- **Database**: PostgreSQL.
- **ORM**: Eloquent (built-in).
- **Validation**: Laravel FormRequest + custom rules.
- **Testing**: PHPUnit + Pest (optional).
- **Task Scheduling / Queue**: Redis + Laravel Queue + Scheduler.
- **API Docs (optional)**: Swagger or Scribe.

## Design Principles
- **Clean Architecture**: separate concerns — Controllers, Services, Repositories, DTOs.
- **SOLID** principles.
- **Convention over Configuration** (Laravel default).
- **Type safety when possible** (PHP 8 typed properties & return types).
- **Fail loud and early** — use strict validation and exception handling.

## Laravel Starter Kit
- Use **Laravel Breeze (Inertia + React)** as the baseline.
  - Provides auth scaffolding, session, basic UI
- Optional: **Jetstream** if team/collaboration features needed (teams, profiles)

## Database
- Use **PostgreSQL** as the primary database.
- Use strong migration conventions:
  - snake_case table names
  - UUID primary keys (when relevant)
  - Soft deletes & timestamps by default
- Prefer explicit columns over json blobs unless necessary.

## Routing
- Group routes by functionality.
- Use API versioning (`api/v1`, `api/v2`, etc).
- Naming routes with meaningful names (`users.index`, `orders.store`).

## Controllers & Logic Separation
- Controllers: only receive requests, validate, call services, return responses.
- Use **FormRequest** classes for validation.
- Move business logic to **Service** layer.
- Repositories handle database querying (optional but recommended for complex apps).

## Validation
- Use FormRequest for all inputs (`StoreUserRequest`, `UpdateOrderRequest`).
- Centralize reusable rules.
- Use custom rule objects instead of inline closures.

## API & Inertia Responses
- API responses:
  - Always return consistent JSON with status & message & data.
  - Use Laravel Resources/Resource Collections.
- Inertia pages:
  - Share necessary props via controllers or Inertia middleware.
  - Keep API-like structure where possible.

## Frontend (React + Inertia)
- Use a global layout component (Nav, Footer).
- Prefer functional components + hooks.
- Use strong typing with TS (optional).
- Centralize API/axios config.
- Use React Query / SWR (optional) for data fetching.

## Authentication & Authorization
- Use Laravel Auth + Sanctum for SPA auth.
- Use Gates & Policies for authorization.
- Centralize permission logic.

## Error Handling
- Use custom exceptions when needed.
- Centralize API error format in `Exceptions/Handler.php`.
- Return consistent error shape.

## Testing
- **Unit tests** for Services, Repositories.
- **Feature tests** for APIs and web flows.
- Use Factories & Seeders to generate test data.
- Optional: Pest for expressive tests.

## Logging & Monitoring
- Use Laravel logging channels (stack, daily).
- Sentry / Bugsnag integration for errors.
- Monitor slow queries with Telescope.

## Queue & Jobs
- Use Redis for queue driver.
- Offload long tasks to Jobs (`SendEmailJob`, `ProcessOrderJob`).

## Deployment Best Practices
- Use Env vars, do not commit `.env`.
- Use configuration caching (`php artisan config:cache`).
- Use route caching (`php artisan route:cache`) in production.
- Use queue workers + supervisor.

## API Documentation
- Use Scribe or Swagger to generate API docs.
- Keep docs updated with annotations.

## CI/CD Recommendations
- Run tests on pull requests.
- Run static analysis (psalm, phpstan).
- Security scan (e.g. vulcan).