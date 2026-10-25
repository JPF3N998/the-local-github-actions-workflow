# hello-world-fastapi Specification

## Purpose
Provides a FastAPI application with integrated ruff linter and formatter for code quality and consistent formatting in the monorepo.

## Requirements

### Requirement: FastAPI app responds to root endpoint

The system SHALL serve a FastAPI application that responds to GET requests at the root path with a greeting message.

#### Scenario: Successful root request

- **WHEN** a GET request is sent to `/`
- **THEN** the system returns HTTP 200 with a JSON body containing a greeting message

#### Scenario: Health check endpoint

- **WHEN** a GET request is sent to `/health`
- **THEN** the system returns HTTP 200 with a JSON body containing the service status

### Requirement: Ruff linter is configured and passes

The system SHALL have a ruff configuration file that defines linting rules, and all source files in the application SHALL pass the linter without errors.

#### Scenario: Linting passes on clean code

- **WHEN** `ruff check` is run on the application source files
- **THEN** the linter reports no errors

#### Scenario: Linting catches violations

- **WHEN** a source file contains a syntax error or unused import
- **THEN** `ruff check` reports the violation with a descriptive message

### Requirement: Ruff formatter is configured and applies

The system SHALL have a ruff configuration that defines formatting rules, and running `ruff format` SHALL produce consistent, formatted source files.

#### Scenario: Formatting applies to source files

- **WHEN** `ruff format` is run on the application source files
- **THEN** all source files are formatted according to the configured rules

#### Scenario: Formatting is idempotent

- **WHEN** `ruff format` is run twice consecutively on already-formatted source files
- **THEN** the second run makes no changes to the files

### Requirement: Application is runnable with uvicorn

The system SHALL provide a way to run the FastAPI application using uvicorn as the ASGI server.

#### Scenario: Application starts successfully

- **WHEN** uvicorn is started with the application entry point
- **THEN** the server listens on the configured host and port
