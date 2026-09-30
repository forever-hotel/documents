# Forever Hotel Database Decision Log

This document records database design issues identified during the
SENG 34213 development phase and the implementation decisions agreed
for the Forever Hotel system.

Architecture-level decisions are documented in the ADR files under:

`adr/`

This decision log records the corresponding database-level implementation
outcomes.

---

## DB-001 - Walk-in Guest Model

### Problem

The original guest model assumes that a registered guest account contains
authentication credentials.

However, the Front Desk System must support walk-in bookings, and a walk-in
guest may not already have a Hotel Website account.

### SRS/SDS Traceability

- SRS: FD-04 - Walk-in Booking
- SDS: Guest and Booking Data Model

### Final Decision

A walk-in booking may be created before a registered guest account exists.

The `bookings.guest_id` field is therefore allowed to be nullable for an
anonymous walk-in booking before guest/account creation is completed.

If a permanent guest account is created later, the booking may be linked to
the corresponding guest record through the approved application workflow.

This avoids forcing the Front Desk System to create Hotel Website credentials
solely to process a walk-in booking.

### Schema Impact

The Booking table permits:

`guest_id = NULL`

for an anonymous walk-in booking prior to guest-account creation.

The existing `guests` table remains the registered guest/account record.

### Ownership

The booking operation belongs to the Front Desk booking domain as defined by
ADR-003.

Shared guest-data changes must follow the coordinated guest-data contract
defined by ADR-003.

### Status

Resolved.

---

## DB-002 - Service Request to Task Relationship

### Problem

A guest or receptionist service request must result in work becoming available
to the Worker Management System.

The database therefore requires an explicit relationship between a service
request and its corresponding worker task.

### SRS/SDS Traceability

- FD-14 - Front Desk service request
- Guest App service-request workflow
- WKMS task queue
- SDS ServiceRequest / Task relationship

### Final Decision

The service-request record and worker task remain separate domain records.

- FOSS owns the service-request record.
- WKMS owns the task record.

`wkms_tasks.service_request_id` provides the database relationship between the
two records.

The relationship is one-to-one for a task generated from a service request.

FDS must not directly create or modify WKMS task persistence.

Cross-subsystem creation must use the approved FOSS/WKMS integration contract.

### Schema Impact

`wkms_tasks` contains a nullable unique:

`service_request_id`

referencing:

`foss_service_requests(request_id)`

### Related ADR

- ADR-003: Database Schema, Ownership, and Migration Strategy
- ADR-004: Real-Time Communication and Event Ownership

### Status

Resolved.

---

## DB-003 - Food Order to Delivery Task Relationship

### Problem

When a KMS food order reaches the state that requires delivery, WKMS needs a
worker task linked to that food order.

### Final Decision

KMS remains authoritative for the food order.

WKMS remains authoritative for the worker task.

A food-delivery task may reference the corresponding KMS food order using:

`wkms_tasks.food_order_id`

The task must be created through the approved KMS/WKMS integration workflow
rather than by another subsystem directly writing WKMS data.

### Schema Impact

`wkms_tasks.food_order_id` references:

`kms_food_orders(order_id)`

The relationship is unique for the specialised food-delivery task.

### Status

Resolved.

---

## DB-004 - Front Desk Identity Verification Persistence

### Problem

FD-05 requires Front Desk staff to verify guest identity during check-in by:

- verifying a physical document; or
- uploading a scanned copy.

The existing Guest and Booking entities do not provide sufficient persistence
for recording who performed the verification, when it occurred, or which
verification method was used.

### Final Decision

Front Desk identity-verification evidence is stored separately in:

`fds_id_verifications`

The record stores verification metadata such as:

- booking;
- document type;
- verification method;
- verifier;
- verification timestamp;
- optional object-storage key;
- optional integrity hash.

The actual scanned NIC/passport file must not be stored directly in PostgreSQL.

Scanned files must be stored in encrypted object storage.

### Schema Impact

The Front Desk physical schema includes:

`fds_id_verifications`

A booking may have one Front Desk identity-verification record for the current
implementation.

### Security Impact

The database stores only verification metadata and an opaque object-storage
reference where required.

Raw identity-document image content is not stored in the relational database.

### Related Requirement

- FD-05

### Status

Resolved.

---

## DB-005 - Booking.lk Import Tracking

### Problem

The Front Desk System imports reservations from Booking.lk.

The system needs to prevent duplicate imports and maintain traceability between
the external reservation reference and the internal booking.

### Final Decision

Booking.lk import metadata is stored in:

`fds_booking_imports`

The existing `bookings` table remains the authoritative hotel reservation
record.

The import table records the external reference, import status, local booking
mapping, timestamps, and safe diagnostic metadata.

Raw Booking.lk payloads containing guest PII must not be stored unnecessarily
in this table.

### Schema Impact

A unique constraint is maintained on:

`provider + external_booking_ref`

to provide database-level protection against duplicate import records.

### Status

Resolved.

---

## DB-006 - Database Hosting

### Problem

The approved SDS originally specifies PostgreSQL as a Docker container hosted
on the application VPS.

The implementation has selected Neon-managed PostgreSQL instead.

### Final Decision

Forever Hotel will use Neon-managed PostgreSQL according to ADR-002.

The project maintains separate logical database environments for:

- development;
- staging;
- production.

Production credentials must not be used for development, CI, or automated
testing.

### Related ADR

- ADR-002: Use Neon-Managed PostgreSQL for Forever Hotel

### Status

Proposed pending required ADR review.

---

## DB-007 - Database Ownership and Cross-Service Writes

### Problem

The approved design contains inconsistent wording regarding shared database
usage and per-service database/schema isolation.

The implementation requires one unambiguous ownership model.

### Final Decision

The project uses one shared PostgreSQL database per environment with explicit
logical domain ownership.

Each business domain has an authoritative owner.

A subsystem must not directly perform operational writes to tables owned by
another subsystem.

Cross-subsystem state changes use approved REST or RabbitMQ contracts.

Read-only cross-domain access may be permitted only where specifically
documented.

### Related ADR

- ADR-003: Database Schema, Ownership, and Migration Strategy

### Status

Proposed pending required ADR review.

---

## DB-008 - Canonical Schema and Migration Location

### Problem

Subsystem repositories must not maintain conflicting copies of the complete
project database schema.

A canonical schema and migration location is required.

### Final Decision

The shared `infra` repository will become the canonical runtime location for:

- the project-wide physical database schema;
- database migration scripts;
- shared database infrastructure configuration.

The `documents` repository remains responsible for:

- database decisions;
- architecture documentation;
- schema design documentation;
- ADRs.

Subsystem repositories may contain their own repository/data-access code but
must not become competing sources of truth for the complete physical schema.

### Migration Rule

All schema changes must be version-controlled.

Automatic production schema synchronization such as:

`synchronize=true`

is prohibited.

### Related ADR

- ADR-003: Database Schema, Ownership, and Migration Strategy

### Status

Proposed pending required ADR review.

---

## DB-009 - Front Desk Folio Persistence

### Problem

FD-09 and FD-15 require Front Desk staff to view an itemised running and
checkout folio.

The normalized database model does not define Folio as a separate logical base
entity.

### Final Decision

A separate `folios` base table will not be introduced at this stage.

The Front Desk application derives the running and checkout folio from
authoritative financial data including:

- room charges;
- food-order charges;
- applicable service charges;
- payments;
- outstanding balance.

KMS remains authoritative for food-order state and charge information.

FDS must obtain external-domain financial information through approved read
contracts.

### Related Requirements

- FD-09
- FD-15

### Related ADR

- ADR-003

### Status

Resolved for the current implementation.

---

## Review Summary

| Decision | Status |
| --- | --- |
| DB-001 Walk-in Guest Model | Resolved |
| DB-002 ServiceRequest -> Task | Resolved |
| DB-003 FoodOrder -> Delivery Task | Resolved |
| DB-004 FDS Identity Verification | Resolved |
| DB-005 Booking.lk Import Tracking | Resolved |
| DB-006 Neon Database Hosting | Proposed - ADR review required |
| DB-007 Database Ownership | Proposed - ADR review required |
| DB-008 Canonical Schema/Migrations | Proposed - ADR review required |
| DB-009 Front Desk Folio Persistence | Resolved |

## Related Artefacts

- ADR-002
- ADR-003
- ADR-004
- DDP-57 / `forever-hotel/documents#9`
- Forever Hotel approved SRS
- Forever Hotel approved SDS
- Current corrected PostgreSQL schema