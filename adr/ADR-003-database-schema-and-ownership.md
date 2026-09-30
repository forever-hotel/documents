# ADR-003: Database Schema, Ownership, and Migration Strategy

## Status

Proposed

## Date

2026-09-30

## Context

Forever Hotel consists of six functional subsystems:

- Hotel Website (HW)
- Manager Dashboard (MAD)
- Front Desk System (FDS)
- Food Ordering and Service Request Application / Guest App (FOSS)
- Kitchen Management System (KMS)
- Worker Management System (WKMS)

The project uses PostgreSQL as its relational database technology.

ADR-002 proposes Neon-managed PostgreSQL as the database hosting platform.

Before persistent subsystem integration continues, the team must define:

- whether the system uses one shared PostgreSQL database or separate databases;
- how subsystem data ownership is enforced;
- which service is allowed to modify each domain;
- where the canonical physical schema is maintained;
- how database migrations are coordinated;
- whether one subsystem may directly modify another subsystem's data.

The approved SDS contains an internal inconsistency regarding this decision.

The high-level architecture and maintainability sections describe a single
PostgreSQL database shared by all six microservices with logical table-prefix
separation. Under that description:

- each service writes only its owned tables;
- reporting and analytics may perform controlled read-only cross-service queries;
- schema changes are coordinated;
- migrations are version controlled.

However, the SDS software-interface specification also describes a dedicated
PostgreSQL schema per microservice and states that cross-service SQL joins are
not permitted.

The current project SQL implementation uses one physical PostgreSQL schema with
logical ownership represented primarily through table naming conventions such
as:

- `mad_*`
- `foss_*`
- `kms_*`
- `wkms_*`
- `fds_*`

and several core operational tables without subsystem prefixes.

This ADR resolves the ambiguity for the development implementation.

## Decision

Forever Hotel will use one shared PostgreSQL database instance per environment.

The database environments are defined by ADR-002:

- development;
- staging;
- production.

The production implementation will use the Neon-managed PostgreSQL environment
defined in ADR-002.

The project will use logical table/domain ownership rather than separate
PostgreSQL database instances for every subsystem.

The current physical SQL schema remains the project database baseline.

Subsystems must not create independent copies of shared business data.

## Ownership Model

Every table or business domain must have one authoritative owner.

An owning service is responsible for:

- validation of writes;
- business-state transitions;
- transaction boundaries;
- repository/data-access implementation;
- migrations affecting the owned table;
- publishing integration events when other subsystems need to react.

Other subsystems must not directly perform operational writes to tables owned
by another service.

Cross-subsystem operational changes must use:

- an approved REST contract; or
- an approved RabbitMQ event contract.

Read access to another domain may be permitted only when specifically
documented and when it does not bypass business rules.

## Domain Ownership

The following ownership model is adopted.

### Front Desk System

FDS is the authoritative operational owner of the hotel reservation and room
operations domain.

FDS owns operational writes for:

- `bookings`
- `payments`
- `room_types`
- `room_type_amenities`
- `room_type_images`
- `rooms`
- `fds_booking_imports`
- `fds_id_verifications`

Responsibilities include:

- booking lookup and reservation operations;
- Booking.lk reservation import;
- walk-in booking creation;
- room assignment;
- room-status changes;
- check-in and check-out state transitions;
- room changes;
- maintenance blocking;
- front-desk cash/card payment recording;
- identity-verification evidence.

Hotel Website and other subsystems that require reservation or room changes
must use an approved FDS API or integration contract instead of directly
modifying these tables.

### Guest / Shared Guest Data

The `guests` table is treated as shared guest master data.

Because both Hotel Website and Front Desk workflows require guest creation or
updates, guest write operations must be performed through an agreed guest-data
application contract rather than through uncontrolled SQL from multiple
services.

Until a separate guest service is introduced, the implementation must ensure
that all guest writes use common validation rules and that changes are
coordinated between Hotel Website and Front Desk.

This table must not be independently redefined by individual subsystem
repositories.

### Manager Dashboard

MAD owns:

- `mad_promotion_codes`
- `mad_promotion_room_types`

MAD is responsible for promotion creation, modification, activation,
deactivation, validity, and redemption-related management rules.

Other subsystems may consume promotion information through approved contracts
but must not directly modify MAD-owned promotion tables.

### Authentication / Staff Management

The `staff_users` table represents staff identity and role information.

Staff account creation, role changes, activation/deactivation, and credential
management belong to the centralized staff-authentication/Manager Dashboard
flow.

FDS, KMS, and WKMS may reference staff identifiers but must not directly
modify staff-account identity or credential data.

### FOSS

FOSS owns:

- `foss_sessions`
- `foss_service_requests`
- `foss_complaints`

FOSS is responsible for:

- guest room-linked session state;
- guest service-request records;
- guest complaint records.

FDS may initiate or request operations related to these domains only through
approved service or event contracts.

For example:

- FDS check-in may request FOSS session activation;
- FDS check-out may request FOSS session deactivation;
- FDS may create a guest service request through an approved integration
  contract.

FDS must not bypass FOSS business logic by directly modifying these tables.

### KMS

KMS owns:

- `kms_meal_categories`
- `kms_menu_items`
- `kms_allergens`
- `kms_menu_item_allergens`
- `kms_dietary_labels`
- `kms_menu_item_dietary_labels`
- `kms_food_orders`
- `kms_food_order_items`
- `kms_kot_prints`

KMS is the authoritative owner of menu, kitchen food-order state, allergen
acknowledgement, stock status, and KOT persistence.

Other subsystems must not directly modify KMS-owned tables.

### WKMS

WKMS owns:

- `wkms_tasks`

WKMS is the authoritative owner of worker-task state.

WKMS is responsible for:

- task creation after an approved triggering event;
- task queue state;
- worker assignment;
- task claiming;
- task progress;
- escalation state;
- completion state.

FDS must not directly update `wkms_tasks` when:

- manually assigning a worker;
- creating a service-related task;
- creating a checkout-cleaning task.

Those operations must be performed through WKMS REST/event contracts.

### Audit Log

`audit_logs` is a system-wide append-only audit table.

It is not treated as the private business table of one subsystem.

Authorized services may append audit events through the approved audit-logging
mechanism.

Application database roles must not receive UPDATE or DELETE permission on
audit records.

Front Desk must append relevant events including:

- check-in;
- check-out;
- room change;
- maintenance action where applicable;
- task assignment/override;
- security-relevant data access.

## Front Desk Database Boundary

For the Front Desk System, database access is therefore divided into two
categories.

### FDS-Owned Data

FDS may implement repositories and transactions for:

- bookings;
- payments;
- rooms and room types;
- Booking.lk import tracking;
- identity-verification metadata.

### External-Owned Data

FDS must access the following through approved integration boundaries:

- FOSS sessions;
- FOSS service requests;
- WKMS tasks;
- KMS food-order information required for folio calculation;
- staff-authentication/account changes.

The presence of foreign keys in the shared physical database does not grant
business ownership to FDS.

Database relationships exist for integrity.

Business ownership is defined by this ADR and by subsystem contracts.

## Folio Decision

A separate `folios` base table will not be introduced at this stage.

The running and checkout folio is a derived view of applicable financial
information, including:

- room charges;
- food-order charges;
- service charges where applicable;
- recorded payments;
- outstanding balance.

The Front Desk application will construct the folio through approved data/API
contracts.

If a future requirement requires immutable issued-invoice persistence, that
must be handled as a separate reviewed schema decision.

## Canonical Database Schema

The complete project-wide physical database schema must have one canonical
source of truth.

The canonical schema and project-wide migration coordination belong to the
shared `infra` repository.

The `documents` repository may contain:

- architecture decisions;
- database design documentation;
- diagrams;
- reviewed schema references;
- migration policy documentation.

It must not become a competing runtime migration source.

Subsystem repositories may contain subsystem-specific entity/repository code,
but they must not maintain independent copies of the complete project schema.

## Migration Strategy

All database schema changes must use version-controlled migration scripts.

The following rules apply:

1. ORM automatic production schema synchronization is prohibited.

2. `synchronize=true` or equivalent automatic destructive production schema
   synchronization must not be used.

3. Every schema change must be represented by a migration.

4. A migration must identify the owning subsystem/domain.

5. Cross-domain changes require coordination with all affected owners.

6. Migrations must be tested against an isolated development/test database
   before staging or production.

7. Production migrations must not be tested directly against production data.

8. Migration execution order must be deterministic.

9. Applied migrations must be traceable.

10. Rollback or recovery implications must be documented for destructive
    changes.

## Database Access Rules

Subsystem database credentials should follow least-privilege principles.

Where practical, database roles should restrict write access according to the
ownership model documented in this ADR.

No subsystem may assume that having technical database connectivity gives it
permission to modify all tables.

Direct cross-service writes are prohibited unless a future ADR explicitly
changes this decision.

## Read-Only Cross-Domain Access

Read-only cross-domain access may be permitted for:

- reporting;
- analytics;
- specifically approved operational aggregation.

Such access must:

- not modify external-owned data;
- not bypass authorization;
- not expose unnecessary PII;
- be documented;
- use parameterized queries;
- remain replaceable by an API/contract where coupling becomes problematic.

Operational state changes must still go through the authoritative owning
service.

## Security Requirements

Database implementation must preserve the security requirements defined by the
approved design.

This includes:

- no database credentials committed to GitHub;
- parameterized database queries;
- PII protection;
- no raw payment-card storage;
- encrypted storage for sensitive identity documents;
- append-only audit logging;
- environment separation;
- least-privilege access.

NIC/passport scan files must not be stored directly in PostgreSQL.

Only approved metadata such as an opaque encrypted-object-storage key may be
stored in the FDS verification record.

## Rationale

The selected strategy was chosen because it:

- matches the current project-wide physical schema;
- preserves one shared PostgreSQL deployment target;
- avoids six duplicated database clusters;
- provides clear service ownership;
- reduces uncontrolled cross-service writes;
- maintains relational integrity between system entities;
- supports the current academic-project deployment scale;
- preserves the possibility of future service/database separation;
- provides a clear migration path from the approved design to implementation.

## Alternatives Considered

### Alternative 1: One completely shared database with unrestricted writes

Under this model, every service could directly read and modify any table.

This option was rejected because it would:

- create strong service coupling;
- allow business rules to be bypassed;
- make ownership unclear;
- make migrations difficult to coordinate;
- reduce independent-service maintainability.

### Alternative 2: Separate PostgreSQL database instance per subsystem

Under this model, every subsystem would maintain an independent PostgreSQL
database.

Advantages:

- strongest physical service isolation;
- clear technical data ownership;
- independent database evolution.

Disadvantages:

- greater operational complexity;
- duplication of shared-reference information;
- more complex cross-service reporting;
- more infrastructure to manage during the academic project;
- does not match the current shared physical schema.

This option is not selected for the current implementation.

### Alternative 3: One shared PostgreSQL database with logical ownership

Under this model:

- one PostgreSQL database is deployed per environment;
- data remains physically relational;
- each business domain has one authoritative writer;
- integrations use REST/events for cross-domain state changes;
- read-only cross-domain reporting may be explicitly permitted.

This option is selected.

## Consequences

As a consequence of this decision:

1. All subsystems connect to the same environment-specific PostgreSQL database.

2. The presence of one shared database does not mean every service owns every
   table.

3. Front Desk implementation must replace temporary in-memory persistence with
   repositories against its approved database domain.

4. Front Desk must not directly update FOSS, KMS, or WKMS operational state.

5. Hotel Website must coordinate reservation/room writes with the authoritative
   reservation domain instead of creating an independent booking schema.

6. Database migrations require project-level coordination.

7. The `infra` repository must be expanded to contain the canonical database
   schema/migration infrastructure.

8. Existing database-decision documentation must be updated so that pending
   decisions already resolved by the final schema are not left marked as
   unresolved.

9. The final SDS must be updated to remove the current contradiction between:

   - shared database with logical table ownership; and
   - dedicated per-microservice PostgreSQL schema/no-cross-service-join wording.

10. Future movement to database-per-service remains possible, but it requires a
    separate architecture decision and data migration plan.

## Compliance and Traceability

### Development Issue

- DDP-57
- `forever-hotel/documents#9`

### Related ADRs

- ADR-001: Subsystem-Oriented Repository Architecture
- ADR-002: Use Neon-Managed PostgreSQL for Forever Hotel

### SRS / Requirement References

This ADR supports database implementation for all subsystems.

For Front Desk specifically, it supports requirements including:

- FD-02 – daily arrivals and departures;
- FD-03 – booking search;
- FD-04 – walk-in bookings;
- FD-05 – identity verification;
- FD-06 – room assignment and room state;
- FD-09 – checkout folio/final payment;
- FD-10 – checkout integration with FOSS and WKMS;
- FD-11 – room changes;
- FD-12 – room status;
- FD-13 – maintenance blocking;
- FD-14 – service requests;
- FD-15 – running folio;
- FD-16 – audit logging;
- FD-18 – WKMS task assignment.

### SDS References

Relevant approved SDS areas include:

- Chapter 1 – Architectural Design
- Section 1.1 – High-Level Architecture
- Section 1.5.2 – Maintainability
- Chapter 2 – Database Design
- Internal Platform Software Interfaces – PostgreSQL
- Chapter 6 – Security Design
- Chapter 7 – Deployment Design

### SENG 34213 Guideline

This ADR supports the requirement that implementation decisions remain
traceable to the approved SRS/SDS.

Because this ADR resolves an inconsistency in the approved SDS and freezes the
implementation architecture, it requires team review and supervisor review.

## Review Information

### Required Review

This ADR requires:

- peer/team architecture review;
- database-owner review by affected subsystem developers;
- supervisor review;
- resolution of blocking review comments.

The status must remain:

`Proposed`

until the required review is complete.

After approval, the status may be changed to:

`Accepted`

## Review Record

| Role | Name | Date | Decision |
| --- | --- | --- | --- |
| Peer Reviewer | Pending | Pending | Pending |
| FDS Owner | Pending | Pending | Pending |
| FOSS Owner | Pending | Pending | Pending |
| KMS Owner | Pending | Pending | Pending |
| WKMS Owner | Pending | Pending | Pending |
| Supervisor | Pending | Pending | Pending |

## Related Artefacts

- Forever Hotel Final Design Report
- Approved Forever Hotel SDS
- Shared corrected PostgreSQL schema
- `database/database-decisions.md`
- ADR-001
- ADR-002
- DDP-57 / documents#9

## Current Decision Status

Proposed