# Kitchen Management System Integration Contract Inventory

## Status

Proposed

## Development Tracking

- KMS Backlog: KMS-#2
- Kitchen Management Issue:
  `forever-hotel/kitchen-management#19`
- Documents Issue:
  `forever-hotel/documents#13`

## Purpose

This document records the integration boundaries required by the Kitchen
Management System.

It defines which subsystem owns each operation, which communication
mechanism is used, and which detailed contracts must be frozen before
implementation.

It follows:

- ADR-003: Database Schema, Ownership, and Migration Strategy
- ADR-004: Real-Time Communication Ownership
- Forever Hotel Standard Backend Folder Structure
- Forever Hotel Standard Frontend Folder Structure

## Communication Rules

- Immediate command/query operations use REST.
- Asynchronous subsystem events use RabbitMQ.
- Realtime UI updates use the shared realtime platform.
- Cross-subsystem operational writes do not use direct SQL.
- Persistent business state is committed before an event/realtime update is
  emitted.
- Integration payloads include only the information required by the
  receiving subsystem.
- Exact paths and event names must be agreed before implementation.

## Contract Inventory

| ID | Integration | Direction | Mechanism | Authoritative Owner | Purpose | Status |
| --- | --- | --- | --- | --- | --- | --- |
| KMS-INT-01 | Menu retrieval | FOSS → KMS | REST | KMS | Obtain menu/category/availability information | Proposed |
| KMS-INT-02 | Food-order submission | FOSS → KMS | To be frozen | KMS | Create an authoritative kitchen food order | Proposed |
| KMS-INT-03 | Order status | FOSS → KMS | REST / realtime as applicable | KMS | Obtain authoritative food-order state | Proposed |
| KMS-INT-04 | Cancellation request/result | FOSS ↔ KMS | To be frozen | KMS | Request and process food-order cancellation | Proposed |
| KMS-INT-05 | Order ready | KMS → WKMS | RabbitMQ | KMS | Trigger worker food-delivery workflow | Required |
| KMS-INT-06 | Stock status change | KMS → MAD | RabbitMQ | KMS | Surface menu-item availability/stock changes | Required |
| KMS-INT-07 | KOT printing | KMS → Printing Service | REST/service contract | KMS | Submit KOT print request | Proposed |
| KMS-INT-08 | Food-order charges | FDS → KMS | REST read | KMS | Supply authoritative order charge information | Required |
| KMS-INT-09 | Kitchen realtime updates | KMS → realtime platform → KMS frontend | Realtime | KMS | Update kitchen queue and order state | Required |

## Contract Details

### KMS-INT-01 — Menu Retrieval

Authoritative owner:

`KMS`

Consumer:

`FOSS`

Purpose:

Allow the guest application to obtain the current menu information required
for food ordering.

Exact endpoint path, query parameters, response DTO, and caching behavior
must be frozen before implementation.

### KMS-INT-02 — Food-Order Submission

Authoritative owner:

`KMS`

Origin:

`FOSS`

The contract must define:

- booking/session reference;
- room reference where required;
- ordered items;
- quantities;
- special notes;
- allergy note;
- idempotency behavior;
- validation errors;
- authoritative order identifier returned by KMS.

The final REST/event mechanism must be agreed before implementation.

### KMS-INT-03 — Order Status

KMS remains authoritative for kitchen order state.

The guest-facing application may query or receive approved status updates,
but must not infer or directly modify KMS persistence.

### KMS-INT-04 — Cancellation

FOSS initiates the guest cancellation request.

KMS evaluates whether cancellation is allowed and performs the authoritative
order-state transition.

The contract must define:

- order identifier;
- cancellation eligibility;
- reason where required;
- success/failure result;
- resulting order state;
- idempotency behavior.

### KMS-INT-05 — Order Ready

Producer:

`KMS`

Consumer:

`WKMS`

Mechanism:

`RabbitMQ`

Purpose:

Notify WKMS when an order is ready for delivery.

The detailed event payload and version must be frozen before implementation.

### KMS-INT-06 — Stock Status Change

Producer:

`KMS`

Consumer:

`MAD`

Mechanism:

`RabbitMQ`

Purpose:

Publish a menu-item stock/availability status change for management
visibility.

### KMS-INT-07 — KOT Printing

KMS requests KOT output through the Printing Service.

KMS does not connect directly to the thermal printer.

The contract must define:

- order identifier;
- KOT identifier;
- printable order data;
- request correlation information;
- duplicate/retry behavior;
- success/failure response.

### KMS-INT-08 — Food-Order Charges

Consumer:

`FDS`

Owner:

`KMS`

Mechanism:

`REST read`

The contract provides the authoritative food-order information required for
running/checkout folio calculation.

This is a read integration only.

### KMS-INT-09 — Kitchen Realtime Updates

KMS publishes realtime information required by the kitchen UI after the
corresponding authoritative state change succeeds.

Relevant categories include:

- new orders;
- order-state changes;
- cancellation;
- menu/stock changes where required.

The frontend does not create an independent WebSocket connection per feature.

## Versioning

Detailed REST and event contracts must be versioned so that breaking changes
cannot be introduced silently.

The final versioning convention will be applied consistently to all approved
KMS contracts.

## Review Required

This inventory remains `Proposed` until reviewed by the affected subsystem
owners.

Required reviewers include, as applicable:

- KMS owner
- FOSS owner
- WKMS owner
- MAD owner
- FDS owner
- shared platform/Printing Service owner