# ADR-001: Subsystem-Oriented Repository Architecture

## Status

Proposed

## Date

2026-09-26

## Context

Forever Hotel is an integrated hotel management system composed of multiple
functional subsystems.

The main subsystem repositories are:

- `hotel-website`
- `manager-dashboard`
- `front-desk`
- `food-ordering`
- `kitchen-management`
- `worker-management`

The project also maintains shared repositories for:

- `api-gateway`
- `infra`
- `tests`
- `documents`

The SENG 34213 development guideline presents a baseline repository structure
with separate frontend, backend, infrastructure, testing, and documentation
repositories.

During development, the team selected a subsystem-oriented repository structure
instead.

Each subsystem repository may contain both:

- `frontend/`
- `backend/`

This allows a subsystem to be developed and maintained as a complete functional
unit while shared responsibilities remain separated into dedicated repositories.

This ADR documents and justifies that architectural decision.

## Decision

The Forever Hotel project will use a subsystem-oriented repository architecture.

Each major business subsystem will have its own GitHub repository.

Where a subsystem contains both client-side and server-side implementation,
the repository will contain both:

```text
subsystem/
├── frontend/
└── backend/
```

Shared technical concerns will remain in separate repositories.

The organisation-level repository structure is:

```text
forever-hotel/
├── hotel-website
├── manager-dashboard
├── front-desk
├── food-ordering
├── kitchen-management
├── worker-management
├── api-gateway
├── infra
├── tests
└── documents
```

## Repository Responsibilities

### `hotel-website`

Contains the implementation of the public-facing hotel website.

### `manager-dashboard`

Contains the implementation of hotel management and administrative functions.

### `front-desk`

Contains the frontend and backend implementation of Front Desk operations.

### `food-ordering`

Contains the frontend and backend implementation of the Food Ordering and
Service Request / Guest App functionality.

### `kitchen-management`

Contains the frontend and backend implementation of Kitchen Management
functionality.

### `worker-management`

Contains the frontend and backend implementation of Worker Management
functionality.

### `api-gateway`

Contains shared API gateway responsibilities used to route and coordinate
requests between system components where applicable.

### `infra`

Contains shared infrastructure and deployment-related configuration, including
where applicable:

- Docker configuration
- Docker Compose configuration
- database infrastructure
- deployment configuration
- shared infrastructure scripts

### `tests`

Contains cross-subsystem integration and end-to-end test assets where those
tests are not appropriately co-located with an individual subsystem.

### `documents`

Contains project-wide engineering documentation, including:

- architectural decision records
- engineering standards
- updated SRS and SDS artefacts
- testing documentation
- security evidence
- sprint documentation
- development reports and supporting evidence

## Rationale

The subsystem-oriented structure was selected because Forever Hotel is designed
as a set of clearly separated functional subsystems.

The approved SDS defines the system as a microservices-based architecture
consisting of six independently deployable subsystems. Each subsystem addresses
a distinct operational domain and communicates using defined API contracts.

The repository structure follows the same subsystem boundaries during
implementation.

Keeping the frontend and backend of the same subsystem in one repository:

1. Keeps closely related functionality together.
2. Supports end-to-end ownership of each subsystem.
3. Reduces coordination overhead for changes affecting both frontend and backend
   within the same subsystem.
4. Makes subsystem boundaries easier to understand.
5. Supports independent subsystem development.
6. Keeps issue, branch, pull request, code, and documentation traceability within
   the same subsystem repository.
7. Avoids coordinating every subsystem change across one organisation-wide
   frontend repository and one organisation-wide backend repository.

Shared concerns that apply across subsystem boundaries remain in dedicated
repositories.

## Alternatives Considered

### Alternative 1: Separate Organisation-Wide Frontend and Backend Repositories

Example:

```text
frontend/
backend/
infra/
tests/
documents/
```

This structure is close to the baseline structure described in the SENG 34213
development guideline.

It was not selected because the Forever Hotel system consists of several
independent operational subsystems.

Using a single organisation-wide frontend repository and a single
organisation-wide backend repository would:

- place unrelated subsystem functionality in the same repositories
- reduce clarity of subsystem ownership
- increase coordination between team members
- require cross-repository changes for many end-to-end subsystem features

### Alternative 2: Single Monorepository for the Entire System

Example:

```text
forever-hotel/
├── frontend/
├── backend/
├── infra/
├── tests/
└── documents/
```

This option would keep the entire system in one repository.

It was not selected because Forever Hotel contains multiple independently
deployable subsystems with separate operational responsibilities.

A single repository would increase coupling between subsystem development
activities and make independent ownership less explicit.

### Alternative 3: Separate Frontend and Backend Repository per Subsystem

Example:

```text
front-desk-frontend
front-desk-backend
worker-management-frontend
worker-management-backend
...
```

This option would provide strong technical separation between frontend and
backend applications.

It was not selected because it would substantially increase the number of
repositories and create additional coordination overhead for features requiring
both frontend and backend changes within the same subsystem.

## Advantages

The selected architecture provides:

- clear subsystem ownership
- end-to-end subsystem development
- reduced cross-repository coordination for subsystem features
- clearer business-domain separation
- easier mapping between GitHub issues and implementation
- independent subsystem development
- separation of shared infrastructure and documentation concerns
- alignment between repository boundaries and the subsystem boundaries defined
  in the approved SDS

## Trade-offs and Risks

The selected structure introduces several trade-offs.

### Configuration Duplication

Different subsystem repositories may contain similar:

- frontend configuration
- backend configuration
- linting configuration
- formatting configuration
- testing configuration
- CI configuration

To reduce inconsistency, shared development standards must be maintained in the
`documents` repository.

### Cross-Subsystem Changes

Features involving multiple subsystems may require coordinated changes across
more than one repository.

Such changes should be managed using:

- linked GitHub issues
- documented API contracts
- coordinated pull requests
- integration testing

### Shared Standards

Because subsystem repositories are maintained independently, project-wide
standards are required for:

- branch naming
- commit messages
- code review
- testing
- CI/CD
- environment management
- documentation
- security

### Dependency and Version Coordination

Shared libraries, API contracts, infrastructure changes, and database changes
may require coordinated updates across several subsystem repositories.

These changes should be tracked explicitly and tested through integration
workflows.

## Consequences

As a result of this decision:

1. Existing subsystem repositories will not be reorganised into one
   organisation-wide frontend repository and one organisation-wide backend
   repository.

2. Each subsystem repository may contain its own:

   - frontend application
   - backend application
   - dependencies
   - tests
   - CI workflows
   - README
   - CHANGELOG
   - environment examples

3. Shared infrastructure remains in the `infra` repository.

4. Cross-subsystem integration and end-to-end tests may be maintained in the
   `tests` repository where they are not suitable for an individual subsystem.

5. Shared development standards and ADRs are maintained in the `documents`
   repository.

6. The `api-gateway` repository remains responsible for shared gateway concerns.

7. Future repositories or significant deviations from this structure must be
   documented and justified.

## Compliance and Traceability

### SENG 34213 Guideline

This ADR supports compliance with the SENG 34213 System Development Project
guideline, including:

- Section 1.3 – Project Continuity
- Section 3.1 – Code Repository Structure
- Section 3.2 – Branch Strategy
- Chapter 4 – GitHub Project Management
- Appendix A – Definition of Done
- Appendix C – GitHub Sprint Checklist

The course guideline provides a baseline repository structure and requires
significant design deviations during development to be documented using an ADR.

### SRS Reference

N/A – this ADR records an engineering architecture and repository-management
decision rather than implementing an individual functional requirement.

However, the repository boundaries support the six-subsystem system scope
defined in the approved project requirements.

### SDS Reference

This decision is aligned with the approved System Design Specification (SDS),
included as Appendix C of the Final Design Report.

Relevant SDS sections are:

- Chapter 1 – Architectural Design
- Section 1.1 – High-Level Architecture
- Section 1.3 – Justification for Architecture Choice
- Section 1.5 – Scalability and Maintainability Considerations
- Section 1.5.2 – Maintainability

The SDS defines Forever Hotel as a microservices-based system comprising six
independently deployable subsystems, each addressing a distinct operational
domain with defined service boundaries and API contracts.

The SDS also identifies independent deployment and maintainability as reasons
for the selected system architecture.

The subsystem-oriented GitHub repository topology documented in this ADR is a
development-phase repository-management decision derived from those approved
architectural boundaries.

The exact GitHub repository topology itself was not explicitly specified in the
approved SDS.

### Development Issue

- DDP-36
- `forever-hotel/documents#7`

### Parent Compliance Issue

- DDP-35
- `forever-hotel/documents#6`

## Relationship to Repository Structure Standard

The implementation rules associated with this ADR are documented in:

```text
standards/repository-structure.md
```

That standard defines the expected internal structure of subsystem repositories
and the responsibilities of shared repositories.

## Review Information

### Required Review

This ADR must receive:

- at least one peer review through the associated pull request
- supervisor review where required by the SENG 34213 development guideline

### Current Decision Status

Current status:

```text
Proposed
```

The ADR must remain `Proposed` until the architectural decision has been
reviewed.

After the required architectural review has been completed, the ADR may be
updated to:

```text
Accepted
```

The review details must then be recorded below.

## Review Record

| Role | Name | Date | Decision |
| --- | --- | --- | --- |
| Peer Reviewer | Pending | Pending | Pending |
| Supervisor | Pending | Pending | Pending |

## Related Artefacts

- SENG 34213 System Development Project guideline
- Forever Hotel Final Design Report
- Forever Hotel approved SDS
- `standards/repository-structure.md`
- DDP-35
- DDP-36