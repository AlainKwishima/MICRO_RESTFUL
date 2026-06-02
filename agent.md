# Project State

Current repository state: a microservices-based institution management starter kit with a workspace layout, shared utilities package, per-service test suites, and a reverse-proxy API gateway.

## High-Level Summary

- The repository has been migrated away from the old monolith into `packages/shared/` plus `services/*`.
- The public entry point is `api-gateway` on port `3000`.
- Domain services exist for auth, users, roles, departments, resources, transactions, notifications, reports, and dashboard aggregation.
- The repo root includes a workspace-aware `package.json`, `docker-compose.yml`, `README.md`, and `scripts/test-all.mjs`.
- Service-level README files are present for every microservice.

## Workspace Layout

- `packages/shared/`
  - Common helpers, types, errors, auth middleware, pagination utilities, HTTP helpers, and shared seed constants.
  - Property-based pagination tests live here.
- `services/api-gateway/`
  - Express reverse proxy that routes `/api/v1/*` prefixes to the downstream services.
- `services/auth-service/`
  - Registration, login, logout, password reset, email verification, and refresh-token flows.
- `services/user-service/`
  - Admin-facing user CRUD.
- `services/role-service/`
  - Role CRUD.
- `services/department-service/`
  - Department CRUD.
- `services/resource-service/`
  - Resource CRUD.
- `services/transaction-service/`
  - Transaction lifecycle, downstream validation, and notification handoff.
- `services/notification-service/`
  - Notification send endpoint plus email template rendering.
- `services/report-service/`
  - Aggregated domain reporting.
- `services/dashboard-service/`
  - Cross-domain dashboard summary.

## What Is Implemented

- Shared `authenticate`, `authorise`, `sendSuccess`, `sendError`, `paginate`, `buildPaginatedResult`, `asyncHandler`, `AppError`, and Zod schemas are centralized in `@shared/lib`.
- Each service has:
  - `src/app.ts`
  - `src/server.ts`
  - `src/config/env.ts`
  - `package.json`
  - `tsconfig.json`
  - `Dockerfile`
  - `jest.config.ts`
  - `.env.example`
- Root orchestration is documented with `docker compose up --build`.
- Root `test:all` runs the shared package and each service test suite.
- Per-service README files document ports, responsibilities, environment variables, and test commands.

## Validation Status

Last known validation results:

- TypeScript builds pass for:
  - `packages/shared`
  - all 10 services
- `npm run test:all` passes.
- Per-service Jest suites pass.
- Gateway routing tests pass.

## Known Technical Gap

- The services are structurally separated and tested, but the current runtime data layer is still in-memory for several services rather than fully Prisma-backed Postgres persistence.
- Prisma schemas are scaffolded, but the repo is not yet fully wired to real migrations, seed execution, and persistent database operations end-to-end.

## Operational Notes

- The root `.env.example` documents the current environment variables for all services.
- The Docker Compose file uses a shared Postgres service and internal service networking.
- The gateway forwards `Authorization` headers unchanged to downstream services.
- Cross-service calls are implemented through HTTP with local timeout handling.

## Useful References

- [Root README](/C:/Users/Alain/Documents/new-projs/microservice_restful/README.md)
- [Workspace package.json](/C:/Users/Alain/Documents/new-projs/microservice_restful/package.json)
- [Docker Compose](/C:/Users/Alain/Documents/new-projs/microservice_restful/docker-compose.yml)
- [Shared library entrypoint](/C:/Users/Alain/Documents/new-projs/microservice_restful/packages/shared/src/index.ts)
- [API gateway app](/C:/Users/Alain/Documents/new-projs/microservice_restful/services/api-gateway/src/app.ts)
