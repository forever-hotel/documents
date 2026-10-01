# Forever Hotel Backend Folder Structure Standard

## Purpose

This document defines the standard backend folder structure for the Forever Hotel Integrated Hotel Management System.

The objective is to keep subsystem backends consistent, maintainable, scalable, and aligned with the agreed subsystem-oriented architecture.

This standard applies primarily to NestJS backend applications used by Forever Hotel subsystem repositories.

---

## Core Principles

1. Organize backend code by business domain or feature, not by frontend client type.
2. A subsystem backend may serve web, mobile, or other clients through the shared API architecture.
3. Shared technical concerns belong in dedicated cross-cutting folders such as `config/`, `database/`, `common/`, `messaging/`, and `realtime/`.
4. Domain-specific controllers, services, DTOs, interfaces, and tests belong inside the owning feature module.
5. Database repositories belong under `database/repositories/`.
6. Shared authentication and authorization are handled centrally through the API gateway architecture and should not be reimplemented independently in each subsystem.
7. Integration and end-to-end tests belong under `test/`.
8. Only create folders and modules that are actually required by the subsystem. Do not add fake implementation files only to make the folder tree appear complete.
9. Keep responsibilities separated so that transport, business logic, persistence, realtime communication, and configuration remain easy to understand and test.
10. New code must follow the project testing and CI requirements.

---

## Standard Backend Structure

```text
backend/
├── src/
│   ├── main.ts
│   ├── app.module.ts
│   │
│   ├── config/
│   │   ├── configuration.ts
│   │   ├── env.validation.ts
│   │   └── constants.ts
│   │
│   ├── database/
│   │   ├── database.module.ts
│   │   ├── database.service.ts
│   │   └── repositories/
│   │       └── <feature>.repository.ts
│   │
│   ├── common/
│   │   ├── decorators/
│   │   ├── dto/
│   │   ├── enums/
│   │   ├── exceptions/
│   │   ├── filters/
│   │   ├── guards/
│   │   ├── interceptors/
│   │   ├── interfaces/
│   │   ├── pipes/
│   │   └── utils/
│   │
│   ├── <feature>/
│   │   ├── dto/
│   │   ├── interfaces/
│   │   ├── <feature>.controller.ts
│   │   ├── <feature>.controller.spec.ts
│   │   ├── <feature>.service.ts
│   │   ├── <feature>.service.spec.ts
│   │   └── <feature>.module.ts
│   │
│   ├── messaging/
│   │   ├── rabbitmq.module.ts
│   │   ├── publishers/
│   │   └── consumers/
│   │
│   ├── realtime/
│   │   ├── realtime.module.ts
│   │   ├── *.gateway.ts
│   │   └── events/
│   │
│   └── health/
│       ├── health.controller.ts
│       └── health.module.ts
│
├── test/
│   ├── integration/
│   └── e2e/
│
├── .env.example
├── Dockerfile
├── README.md
├── package.json
├── tsconfig.json
└── tsconfig.build.json
```

The structure above is a reference. A subsystem should only contain folders that correspond to implemented responsibilities.

---

## `main.ts`

`main.ts` is the application bootstrap entry point.

It should remain focused on application startup concerns such as:

- Creating the NestJS application
- Applying global pipes, filters, or interceptors where required
- Enabling CORS where required by the architecture
- Starting the HTTP server

Business logic should not be implemented in `main.ts`.

---

## `app.module.ts`

`app.module.ts` is the root NestJS module.

Its responsibility is to compose application modules such as:

- Configuration
- Database
- Domain feature modules
- Messaging
- Realtime communication
- Health checks

It should not contain business logic.

---

## `config/`

Application configuration belongs under:

```text
src/config/
```

Recommended files:

```text
config/
├── configuration.ts
├── env.validation.ts
└── constants.ts
```

### `configuration.ts`

Defines structured runtime configuration values loaded from the environment.

### `env.validation.ts`

Validates required environment variables and fails fast when mandatory configuration is missing or invalid.

### `constants.ts`

Contains configuration keys or other configuration-related constants.

Actual secrets must not be committed to source control. Use `.env.example` to document required variables without real secret values.

---

## `database/`

Database infrastructure belongs under:

```text
src/database/
```

Example:

```text
database/
├── database.module.ts
├── database.service.ts
└── repositories/
    ├── task.repository.ts
    └── service-request.repository.ts
```

### `database.module.ts`

Registers and exports database infrastructure required by feature modules.

### `database.service.ts`

Owns the database connection or shared persistence client.

### `repositories/`

Contains persistence logic for domain data.

Repositories should:

- Encapsulate database queries
- Keep SQL or persistence-specific implementation out of controllers
- Keep persistence details separate from business logic
- Support transaction-safe operations where required

Repository tests may be colocated with repository files.

Example:

```text
repositories/
├── task.repository.ts
├── task.repository.spec.ts
├── service-request.repository.ts
└── service-request.repository.spec.ts
```

---

## `common/`

Cross-cutting reusable backend code belongs under:

```text
src/common/
```

Possible subfolders include:

```text
common/
├── decorators/
├── dto/
├── enums/
├── exceptions/
├── filters/
├── guards/
├── interceptors/
├── interfaces/
├── pipes/
└── utils/
```

Only create these folders when shared code actually exists.

Feature-specific DTOs, enums, interfaces, exceptions, or utilities should stay inside the owning feature rather than being moved into `common/` unnecessarily.

---

## Feature Modules

Business functionality should be organized by domain or feature.

Example:

```text
tasks/
├── dto/
├── interfaces/
├── task.controller.ts
├── task.controller.spec.ts
├── task.service.ts
├── task.service.spec.ts
└── task.module.ts
```

### Controller

The controller is responsible for the HTTP/API layer.

Typical responsibilities:

- Define routes
- Accept validated input
- Call the appropriate service
- Return HTTP responses

Controllers should not contain persistence logic or complex business rules.

### Service

The service owns feature business logic and orchestration.

It may:

- Apply business rules
- Coordinate repositories
- Coordinate other services
- Publish events where required
- Return domain results to the controller

### DTOs

Feature request and response DTOs belong under:

```text
<feature>/dto/
```

### Interfaces

Feature-specific interfaces belong under:

```text
<feature>/interfaces/
```

### Module

The feature module defines the NestJS dependency boundary for that domain.

---

## Organize by Domain, Not Client

Backends must be organized around business capabilities rather than around the type of frontend consuming the API.

Avoid structures such as:

```text
web/
mobile/
admin/
```

when the same business domain is shared across clients.

Prefer:

```text
tasks/
workers/
shifts/
service-requests/
```

For example, the `tasks` feature should own task behaviour whether the request originates from a worker mobile interface, manager dashboard, or another approved client.

---

## `messaging/`

Asynchronous messaging infrastructure belongs under:

```text
src/messaging/
```

Recommended organization:

```text
messaging/
├── rabbitmq.module.ts
├── publishers/
└── consumers/
```

### Publishers

Publishers send domain or integration events to the message broker.

### Consumers

Consumers handle events received from other services or subsystems.

Messaging transport code should remain separate from feature business logic.

Only add messaging components when the subsystem actually uses broker-based communication.

---

## `realtime/`

Realtime communication infrastructure belongs under:

```text
src/realtime/
```

Recommended organization:

```text
realtime/
├── realtime.module.ts
├── *.gateway.ts
└── events/
```

This area may contain:

- WebSocket gateways
- Realtime connection management
- Shared realtime event definitions
- Realtime transport configuration

Feature-specific realtime business behaviour should remain owned by the appropriate feature/service and use the shared realtime infrastructure.

---

## `health/`

Deployment and service health endpoints belong under:

```text
src/health/
```

Example:

```text
health/
├── health.controller.ts
└── health.module.ts
```

Health functionality may expose checks required by local development, CI/CD, containers, deployment platforms, or service monitoring.

Do not use a generic scaffold endpoint as a substitute for a real health check when production health functionality is required.

---

## Shared Authentication and Authorization

Forever Hotel uses shared authentication and authorization through the central API architecture.

Subsystem backends should not create separate independent JWT/login systems unless a documented architecture decision explicitly requires it.

Subsystem services should consume trusted authentication/authorization context provided through the approved shared gateway flow.

Examples of centrally managed concerns include:

- Authentication
- Token validation
- Role-based access control
- Common identity context

Feature modules may still enforce feature-specific authorization rules using the trusted user context where required.

---

## Testing Structure

Unit tests should normally be colocated with the code they test.

Example:

```text
tasks/
├── task.controller.ts
├── task.controller.spec.ts
├── task.service.ts
└── task.service.spec.ts
```

Repository tests may be colocated under:

```text
database/repositories/
```

Higher-level tests belong under:

```text
test/
├── integration/
└── e2e/
```

### Integration Tests

Integration tests validate collaboration between application components and API endpoints.

Example:

```text
test/integration/task.controller.integration.spec.ts
```

### End-to-End Tests

End-to-end tests validate the application through its externally visible interfaces.

Example:

```text
test/e2e/app.e2e-spec.ts
```

Where an existing project uses a compatible E2E configuration at another path, it may be migrated when the relevant testing task is implemented.

---

## Testing and Coverage Requirements

The project testing standard requires:

- Unit test coverage of at least 80% for new code
- At least one integration test for each API endpoint
- At least 90% branch coverage for critical business logic
- Coverage reports generated through CI
- Test evidence available for Sprint Review

New implementation should include appropriate tests as part of the same development work rather than postponing testing until the end.

---

## Environment Files

Each backend should document required environment variables using:

```text
.env.example
```

Example:

```env
DATABASE_URL=
RABBITMQ_URL=
```

Only variables actually required by the subsystem should be included.

Never commit:

```text
.env
```

or real credentials, secrets, passwords, private keys, or production tokens.

---

## Worker Management System Example

The Worker Management System may use the following backend domain structure:

```text
worker-management/backend/src/
├── config/
├── database/
│   └── repositories/
├── common/
├── tasks/
├── service-requests/
├── workers/
├── shifts/
├── messaging/
├── realtime/
└── health/
```

Only implemented features need to exist.

For example, an early WKMS implementation may contain:

```text
src/
├── config/
├── database/
│   ├── database.module.ts
│   ├── database.service.ts
│   └── repositories/
│       ├── task.repository.ts
│       └── service-request.repository.ts
├── tasks/
│   ├── task.controller.ts
│   ├── task.service.ts
│   └── task.module.ts
├── app.module.ts
└── main.ts
```

Additional folders such as `workers/`, `shifts/`, `messaging/`, `realtime/`, and `health/` should be added as those capabilities are implemented.

---

## Naming Guidelines

Use lowercase kebab-case for folders and file names.

Examples:

```text
task.controller.ts
task.service.ts
task.module.ts
task.repository.ts
service-request.repository.ts
env.validation.ts
```

Use NestJS suffix conventions consistently:

```text
*.controller.ts
*.service.ts
*.module.ts
*.repository.ts
*.gateway.ts
*.spec.ts
```

Names should reflect the business responsibility of the file.

---

## Dependency Rules

To keep boundaries clear:

1. Controllers should depend on services rather than repositories directly where business logic is involved.
2. Services may depend on repositories and approved shared infrastructure.
3. Persistence code should stay under database/repository layers.
4. Shared transport infrastructure should stay under `messaging/` or `realtime/`.
5. Feature-specific logic should stay inside the owning feature.
6. Avoid circular dependencies between modules.
7. Do not move feature-specific code into `common/` only for convenience.
8. Authentication concerns must follow the shared API gateway architecture.

---

## Consistency Rule

All Forever Hotel subsystem backends should follow this common structure unless a documented architectural decision requires an exception.

When adding new backend functionality:

1. Identify the owning business domain.
2. Add or extend the relevant feature module.
3. Keep controllers thin.
4. Put business rules in services.
5. Put persistence logic in repositories.
6. Use shared configuration and infrastructure.
7. Add unit and integration tests with the implementation.
8. Maintain the required test coverage.
9. Avoid creating unused folders or placeholder implementation files.

---

