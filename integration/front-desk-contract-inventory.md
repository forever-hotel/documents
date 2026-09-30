# Front Desk System Integration Contract Inventory

## Status

Proposed

## Date

2026-09-30

## Development Issue

- DDP-57
- `forever-hotel/documents#9`

## Purpose

This document records the integration boundaries required by the Front Desk
System (FDS).

It is an inventory of required contracts.

Detailed endpoint payloads, event payloads, and error bodies may be defined in
separate contract documents during implementation.

The inventory follows:

- ADR-002: Neon PostgreSQL Hosting
- ADR-003: Database Schema and Ownership
- ADR-004: Real-Time Communication and Event Ownership

## Communication Rules

The following rules apply:

- Browser/client requests use the API Gateway.
- Immediate command/query operations use REST.
- Asynchronous cross-subsystem events use RabbitMQ.
- Real-time UI notifications use WebSocket / Socket.IO.
- FDS must not directly modify tables owned by another subsystem.
- Persistent business state must be committed before a realtime notification
  is emitted.
- RabbitMQ payloads must avoid unnecessary guest PII.
- Exact endpoint paths must be agreed before implementation and must not be
  invented independently by individual subsystem developers.

## Contract Inventory

| ID | Integration | Direction | Mechanism | Authoritative Owner | Purpose | Related Requirement | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FDS-INT-01 | Staff authentication / authorization | Front Desk Client -> API Gateway/Auth -> FDS | HTTPS REST / JWT | API Gateway / Auth | Authenticate receptionist and protect FDS operations | FD-01 | Required |
| FDS-INT-02 | Online booking and room availability | Hotel Website <-> FDS booking/room domain | HTTPS REST via API Gateway | FDS for booking/room operational state | Allow the Hotel Website to use authoritative availability and reservation state | System booking scope | Required |
| FDS-INT-03 | Booking.lk reservation import | Booking.lk -> FDS | External HTTPS REST/API | FDS | Import third-party reservations into the FDS booking domain | Booking.lk integration / FD-03 | Required |
| FDS-INT-04 | Activate guest FOSS session | FDS -> FOSS | HTTPS REST | FOSS | Activate room-linked guest access after successful check-in | FD-07 | Required |
| FDS-INT-05 | Deactivate guest FOSS session | FDS -> FOSS | HTTPS REST | FOSS | Revoke/deactivate guest application access during checkout | FD-10 / GA-01b | Required |
| FDS-INT-06 | Checkout completion | FDS -> RabbitMQ -> WKMS | RabbitMQ event | FDS produces; WKMS consumes | Trigger creation of a High-priority room-cleaning task after successful checkout | FD-10 | Source-defined event |
| FDS-INT-07 | Task escalation alert | WKMS -> RabbitMQ -> FDS | RabbitMQ event | WKMS produces; FDS consumes | Notify FDS when a task remains unclaimed beyond the escalation threshold | FD-17 / FD-19 | Source-defined event |
| FDS-INT-08 | Manual worker assignment | FDS -> WKMS | HTTPS REST | WKMS | Assign an escalated/unclaimed task to a selected available worker and return immediate success/failure | FD-18 | Required |
| FDS-INT-09 | Front-desk service request | FDS -> service-request/task workflow | REST + approved subsystem integration | FOSS/WKMS according to ADR-003 ownership | Create a guest service request from the Front Desk and ensure the corresponding worker task enters WKMS | FD-14 | Contract detail to freeze |
| FDS-INT-10 | Food-order charges for running/checkout folio | FDS -> KMS | HTTPS REST/read contract | KMS | Obtain authoritative food-order charge information required by the Front Desk folio | FD-09 / FD-15 | Required |
| FDS-INT-11 | Front Desk realtime updates | FDS Backend -> API Gateway WSS -> FDS Client | WebSocket / Socket.IO | FDS | Push room-status and Front Desk dashboard realtime updates | FD-12 | Required |
| FDS-INT-12 | Escalation realtime notification | FDS Backend -> API Gateway WSS -> FDS Client | WebSocket / Socket.IO | FDS | Push consumed WKMS escalation information to the receptionist dashboard | FD-17 / FD-19 | Required |

## Contract Notes

### FDS-INT-01 — Authentication and Authorization

The Front Desk frontend must not implement its own independent authentication
authority.

Protected Front Desk requests are routed through the API Gateway and use the
central staff authentication/JWT mechanism.

The FDS backend still enforces the business authorization applicable to its
operations.

## FDS-INT-02 — Hotel Website Booking Integration

The Hotel Website requires room availability and online reservation operations.

The booking and room operational state defined by ADR-003 must remain
authoritative.

The Hotel Website must not maintain an independent competing copy of booking
or room availability state.

Exact request/response contracts must be coordinated with the Hotel Website
owner.

## FDS-INT-03 — Booking.lk Import

Booking.lk reservation imports are handled by FDS.

FDS is responsible for:

- import validation;
- duplicate/idempotency protection;
- mapping the external booking reference to the local booking;
- safe failure handling.

Raw credentials or Booking.lk access secrets must not be committed to GitHub.

## FDS-INT-04 — FOSS Session Activation

After successful check-in, FDS requests activation of the guest's room-linked
FOSS session.

FOSS remains authoritative for the FOSS session itself.

FDS must not directly modify FOSS-owned session persistence.

The command requires an immediate response and therefore uses an approved REST
contract.

## FDS-INT-05 — FOSS Session Deactivation

During successful checkout, FDS requests deactivation of the active guest FOSS
session.

The checkout workflow must not leave an active guest session after the guest is
successfully checked out.

The exact failure/retry behaviour must be defined in the detailed contract.

## FDS-INT-06 — Checkout Completion Event

Producer:

`FDS`

Consumer:

`WKMS`

Mechanism:

`RabbitMQ`

The event is published only after the authoritative FDS checkout state change
has succeeded.

WKMS consumes the event and creates the required High-priority room-cleaning
task.

FDS must not directly insert the cleaning task into `wkms_tasks`.

The detailed event contract must define:

- event type;
- booking identifier;
- room identifier;
- event timestamp;
- correlation identifier;
- idempotency behaviour.

Guest PII must not be included where identifiers are sufficient.

## FDS-INT-07 — Task Escalation Event

Producer:

`WKMS`

Consumer:

`FDS`

Mechanism:

`RabbitMQ`

WKMS detects the authoritative escalation condition.

FDS consumes the event and displays the escalation to Front Desk staff.

The detailed contract must define:

- event type;
- task identifier;
- room identifier where required;
- escalation timestamp;
- correlation identifier.

FDS must not directly change the WKMS escalation state.

## FDS-INT-08 — Manual Worker Assignment

The receptionist requires an immediate success/failure response when assigning
a worker.

Therefore manual assignment uses an approved WKMS REST command contract.

WKMS remains responsible for:

- validating that the task can still be assigned;
- validating the selected worker;
- preventing invalid/concurrent assignment;
- persisting the task assignment.

FDS displays the result returned by WKMS.

## FDS-INT-09 — Front Desk Service Request

The approved functional requirement states that Front Desk staff can create a
service request on behalf of a checked-in guest and submit it to the WKMS task
queue.

The current physical schema and ADR-003 separate:

- ServiceRequest ownership under FOSS; and
- Task ownership under WKMS.

Therefore this contract requires coordinated FOSS/WKMS handling.

The implementation must ensure that:

1. one valid service-request record exists;
2. one corresponding WKMS task is created where required;
3. duplicate task creation is prevented;
4. the correct booking/room is linked;
5. FDS does not directly write FOSS- or WKMS-owned tables.

The exact orchestration path must be approved by the FOSS and WKMS owners
before implementation.

## FDS-INT-10 — Folio Food-Order Charges

The Front Desk running and checkout folio includes applicable food-order
charges.

KMS remains authoritative for food-order state and order amount.

FDS therefore requires a read contract that can obtain the applicable
chargeable orders for the active booking/stay.

The detailed contract must define which food-order states are financially
chargeable and how cancelled/refunded orders are represented.

FDS must not modify KMS food-order state through this contract.

## FDS-INT-11 — Room Status Realtime

Room-state changes are first persisted through the authoritative FDS business
operation.

After the state change succeeds, FDS may emit a realtime Socket.IO event.

Realtime clients must treat REST/server state as authoritative after reconnect
or synchronization failure.

## FDS-INT-12 — Escalation Realtime Notification

The complete escalation notification flow is:

```text
WKMS
  |
  | RabbitMQ task-escalation event
  v
FDS backend consumer
  |
  | Socket.IO / WSS
  v
API Gateway
  |
  v
Front Desk client