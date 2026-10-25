# 0002-hello-world-fastapi

## Status
Implemented

## Context
Set up a basic FastAPI application with integrated ruff linter and formatter in the monorepo's apps/ directory. This provides developers with a complete, ready-to-use FastAPI setup with proper code quality tooling out of the box.

Key constraints and questions:
- The project uses a monorepo structure with an apps/ directory for applications
- The application should be minimal, focused on demonstrating FastAPI + ruff integration
- No authentication, authorization, or database integration required
- Use uv as the package manager for dependency management
- Follow existing documentation patterns in the repo

## Decision
Created a hello-world-fastapi application in `/home/jpfeng/Dev/local-ci-agent/apps/hello-world-fastapi/` with:

1. **FastAPI application**:
   - Basic "Hello World" endpoint at `/` returning {"message": "Hello World"}
   - Health check endpoint at `/health` returning {"status": "healthy"}
   - Minimal FastAPI setup with dependency injection ready

2. **uv-managed dependencies**:
   - pyproject.toml with FastAPI and uvicorn dependencies
   - Dependency installation verified using `uv sync`

3. **Ruff linting and formatting**:
   - `ruff.toml` configuration at `/home/jpfeng/Dev/local-ci-agent/apps/hello-world-fastapi/ruff.toml` (co-located with the FastAPI application)
   - `ruff check` validates code without errors
   - `ruff format` ensures consistent code style

## Consequences

- **Pros**: Developers have a ready-to-use FastAPI setup with modern tooling; code quality enforced; consistent with monorepo structure
- **Cons**: Initial setup effort; requires developer to maintain the hello-world-fastapi directory
- **Related decisions**: CI/CD integration will be added separately (Phase 2)

## Tags
fastapi, ruff, uv, ci-cd, monorepo