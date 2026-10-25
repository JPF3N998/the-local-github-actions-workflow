# Design

## Context

This is a greenfield change within a monorepo where the `apps/` directory exists but is currently empty. The project uses the spec-driven OpenSpec schema and has no existing capabilities or implementation. The project context field in `openspec/config.yaml` is empty, so no constraints were provided there.

## Goals / Non-Goals

**Goals:**

- Provide a basic FastAPI application that serves requests and responds to health checks
- Configure ruff linter with sensible default rules
- Configure ruff formatter for consistent code style
- Enable automated linting and formatting checks through CI (planned for Phase 2)

**Non-Goals:**

- Authentication, authorization, or database integration
- Real business logic or multiple endpoints
- Deployment infrastructure or production configuration

## Decisions

### Decision: Use FastAPI as the web framework

The basic FastAPI framework was chosen because it provides modern Python ASGI support, automatic OpenAPI documentation, and integrates well with uvicorn.

### Decision: Place application code in apps/ directory

The application source will live in the monorepo's `apps/` directory to follow the established structure for multiple applications within this monorepo.

### Decision: Use ruff for both linting and formatting

Ruff was selected as it consolidates linting and formatting into a single fast tool, reducing tooling complexity while providing modern rule sets.

### Decision: Configure default ruff rules and formatter

Ruff configuration (`ruff.toml`) is placed at `/home/jpfeng/Dev/local-ci-agent/apps/hello-world-fastapi/` to colocate linting with the application source.

### Decision: Use uv as package manager

The project will use uv for dependency management instead of pip. uv provides faster dependency resolution, better caching, and a more modern Python packaging experience.

### Decision: Use fastapi[standard] for development, bare fastapi for production

- **Development**: `fastapi[standard]` includes the `fastapi` CLI (`uv run fastapi dev`), uvicorn, pydantic-settings, rich, and other dev helpers
- **Production containers**: Use bare `fastapi` + `uvicorn` only for minimal deployment images

This separation ensures development is ergonomic while production deployments are lean.

The project will use uv for dependency management instead of pip. uv provides faster dependency resolution, better caching, and a more modern Python packaging experience. The FastAPI application dependencies will be managed through uv with a pyproject.toml file.

## Risks / Trade-offs

- **Risk**: Minimal project setup may miss platform-specific linting rules
- **Mitigation**: Start with ruff defaults and expand rule sets based on observed findings.

- **Risk**: FastAPI application may grow quickly without clear architecture boundaries
- **Mitigation**: Keep the initial application intentionally minimal; define module structure when application scope grows.

## Migration Plan

No migration required - this change creates new artifacts for a new application.

## Open Questions

- None - all required decisions are resolved for this foundational change.
