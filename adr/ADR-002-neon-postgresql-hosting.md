# ADR-002: Use Neon-Managed PostgreSQL for Forever Hotel

## Status

Proposed

## Date

2026-09-30

## Context

The approved Forever Hotel System Design Specification (SDS) defines
PostgreSQL as the system database.

The approved deployment design originally places PostgreSQL inside a Docker
container on the production Linux VPS with a persistent volume.

During the development phase, the team selected Neon-managed PostgreSQL
instead of self-hosting the PostgreSQL database container on the application
VPS.

A Neon project named `forever-hotel` has already been created.

The current Neon project contains a default database branch named:

`production`

This changes the database hosting approach defined in the approved SDS.

Under the SENG 34213 development guideline, significant implementation
deviations from the approved SDS must be documented using an Architectural
Decision Record and reviewed with the supervisor.

This ADR therefore records the proposed database-hosting decision before the
Front Desk System and the other subsystems continue their persistent database
integration.

## Decision

Forever Hotel will use Neon-managed PostgreSQL as the managed PostgreSQL
hosting platform for the shared application database.

The existing Neon project named `forever-hotel` will be used as the project-level
database environment.

Database environments will remain separated as follows:

- `production` — production database environment.
- `staging` — integration, UAT, and pre-production validation.
- `development` — shared development and integration database environment.

The existing Neon `production` branch will remain the production database
branch.

Separate Neon branches named `staging` and `development` will be created
before those environments are used.

Production credentials must not be reused by development, staging, CI, or local
developer environments.

Application services will receive database connection details through
environment variables or deployment secrets. Database credentials and complete
connection strings must never be committed to GitHub.

The canonical database schema and migration ownership will be defined separately
in ADR-003.

This ADR changes only the PostgreSQL hosting approach. It does not change:

- the six-subsystem architecture;
- subsystem business responsibilities;
- PostgreSQL as the selected relational database technology;
- RabbitMQ messaging architecture;
- API Gateway responsibilities; or
- the approved functional requirements.

Local automated tests and CI may use isolated test PostgreSQL environments.
They must never execute tests against the production database.

## Rationale

The team selected Neon-managed PostgreSQL because it preserves PostgreSQL as
the approved relational database technology while removing the need for the
development team to operate the database server directly on the application VPS.

This decision provides the following benefits:

- PostgreSQL remains the database technology defined by the approved design.
- Database hosting and application hosting can be managed independently.
- Development, staging, and production database environments can be isolated.
- Database connection information can be supplied through environment variables.
- The team does not need to maintain a PostgreSQL Docker volume on the
  production application VPS.
- The database can be accessed consistently by independently deployed subsystem
  backends.
- The decision reduces infrastructure-management work during the academic
  development period.

The change affects database hosting only. Application services will continue to
treat PostgreSQL as the system's relational data store.

## Alternatives Considered

### Alternative 1: Keep PostgreSQL in Docker on the application VPS

This is the hosting model described in the approved SDS.

Advantages:

- Matches the original SDS deployment design directly.
- Full infrastructure control remains with the project team.
- Database and application infrastructure can be managed in one Docker Compose
  deployment.

Disadvantages:

- The team must manage PostgreSQL availability, storage volumes, backups,
  upgrades, and database-server maintenance.
- Database availability becomes more closely coupled to the application VPS.
- Additional infrastructure management is required during development and
  deployment.

This alternative was not selected for the current implementation.

### Alternative 2: Use a separate self-managed PostgreSQL server

Under this option, PostgreSQL would run on a separate VPS or database host
managed by the project team.

Advantages:

- Separates database infrastructure from the application VPS.
- Maintains full administrative control.

Disadvantages:

- The team remains responsible for server provisioning, operating-system
  maintenance, PostgreSQL configuration, backups, security, and availability.
- It increases operational complexity compared with a managed PostgreSQL
  service.

This alternative was not selected.

### Alternative 3: Use Neon-managed PostgreSQL

Under this option, PostgreSQL is hosted through Neon while application services
connect using securely managed database connection information.

Advantages:

- Retains PostgreSQL compatibility.
- Reduces direct database-server administration.
- Allows isolated database environments.
- Separates database hosting from application-container hosting.
- Fits the current development workflow.

Disadvantages:

- Introduces dependency on an external managed database provider.
- Application services require network access to the managed database.
- Connection, environment, backup, and provider configuration must be reflected
  in the implemented deployment documentation.
- The final SDS deployment design must be updated to reflect the managed
  database architecture.

This alternative is selected.

## Consequences

As a consequence of this decision:

1. PostgreSQL will no longer be deployed as the production database container
   inside the Forever Hotel application VPS deployment.

2. The application services may still be containerized as defined by the system
   architecture, but their PostgreSQL connections will target Neon through
   environment-specific configuration.

3. The production Docker Compose configuration must not assume that a local
   PostgreSQL container is the production source of truth.

4. Database credentials and connection strings must be treated as secrets and
   must not be committed to GitHub.

5. Development, staging, and production database environments must remain
   logically separated.

6. Automated tests must not use the production database.

7. The database schema and migration source of truth must be defined in
   ADR-003.

8. The final System Design Specification must be updated to replace the original
   self-hosted PostgreSQL deployment description with the implemented managed
   PostgreSQL architecture.

9. This ADR requires team review and supervisor review before its status is
   changed from `Proposed` to `Accepted`.

   ## Compliance and Traceability

### Development Issue

- DDP-57
- `forever-hotel/documents#9`

### Related ADR

- ADR-001: Subsystem-Oriented Repository Architecture

### SRS Reference

This ADR does not change a functional requirement.

It changes the implementation/deployment approach used to host the
PostgreSQL database.

### SDS Reference

This ADR affects the following approved SDS areas:

- Chapter 1 – Architectural Design
- Section 1.2 – Cloud-Native and Containerized Architecture
- Section 1.2.1 – Container Deployment Model
- Chapter 7 – Deployment Design
- Section 7.1 – Hosting Plan
- Section 7.4 – Environment Specification

The approved SDS originally specifies PostgreSQL as a Docker container
hosted on the project VPS.

The implemented design replaces that database-hosting component with
Neon-managed PostgreSQL while retaining PostgreSQL as the relational
database technology.

### SENG 34213 Guideline Reference

The SENG 34213 development guideline requires significant deviations from
the approved SDS to:

- be documented using an ADR;
- be reviewed with the supervisor within one sprint; and
- be reflected in the final updated SDS.

## Review Information

### Required Review

This ADR requires:

- team/peer architecture review;
- supervisor review;
- resolution of blocking review comments.

The ADR must remain:

`Proposed`

until the required review is completed.

After approval, the status may be changed to:

`Accepted`

## Review Record

| Role | Name | Date | Decision |
| --- | --- | --- | --- |
| Peer Reviewer | Pending | Pending | Pending |
| Supervisor | Pending | Pending | Pending |

## Related Artefacts

- Forever Hotel Final Design Report
- Approved Forever Hotel SDS
- SENG 34213 System Development Project guideline
- ADR-001
- DDP-57 / documents#9
- `database/database-decisions.md`

## Current Decision Status

Proposed