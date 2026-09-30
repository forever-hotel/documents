# ADR-004: Real-Time Communication and Event Ownership

## Status

Proposed

## Date

2026-09-30

## Context

Forever Hotel uses three communication patterns:

1. synchronous REST communication;
2. asynchronous RabbitMQ event communication;
3. WebSocket Secure (WSS) real-time client updates.

The approved SDS defines:

- REST/HTTPS for client-initiated operations requiring an immediate response;
- RabbitMQ using AMQP 0-9-1 for asynchronous inter-service events;
- WebSocket Secure using Socket.IO/NestJS WebSocket gateways for real-time
  interface updates.

The design specifically identifies events including:

- KMS order ready -> WKMS;
- WKMS task escalation -> FDS;
- KMS stock-status change -> MAD;
- FDS checkout completion -> WKMS.

The design also requires real-time updates for:

- KMS order queue;
- WKMS task queue;
- FDS room-status board;
- FDS escalation panel;
- MAD operational analytics;
- FOSS order/service-request tracking.

However, the approved SDS describes a central WebSocket server container while
the current subsystem backend standard provides a `realtime/` module inside
each subsystem backend.

The current GitHub organisation also contains no dedicated WebSocket-server
repository.

The team therefore needs to define:

- who owns RabbitMQ infrastructure;
- who owns RabbitMQ event contracts;
- who publishes and consumes each event;
- who owns Socket.IO/WebSocket gateways;
- how the API Gateway participates in WebSocket communication;
- how FDS receives WKMS escalation information;
- how FDS publishes checkout-trigger information;
- how real-time updates reach the Front Desk frontend.

This ADR resolves those ownership boundaries.

## Decision

Forever Hotel will maintain the three communication mechanisms for their
different purposes.

### REST

REST is used for:

- commands requiring an immediate response;
- user-initiated operations;
- direct resource queries;
- operations where the caller must immediately know whether the command
  succeeded or failed.

Client-facing requests must continue to pass through the API Gateway according
to the approved architecture.

### RabbitMQ

RabbitMQ is used for asynchronous cross-subsystem events.

It must not be used as a replacement for every REST operation.

### WebSocket / Socket.IO

WebSocket Secure is used only for real-time push updates to connected client
interfaces.

A WebSocket event must not become the authoritative persistence mechanism for
business state.

Persistent state must first be committed by the owning backend/domain.

The resulting state/update may then be pushed to connected clients.

## Infrastructure Ownership

### RabbitMQ Infrastructure

The shared `infra` repository owns RabbitMQ infrastructure configuration.

This includes, where applicable:

- broker deployment configuration;
- durable exchange configuration;
- queue infrastructure conventions;
- dead-letter configuration standards;
- environment configuration;
- health/readiness configuration.

Business event publishing and consumption remain inside the relevant subsystem
backends.

### API Gateway

The API Gateway remains the external entry boundary.

For WebSocket traffic, it is responsible for:

- accepting/proxying the WSS upgrade path;
- routing approved WebSocket traffic to the appropriate backend realtime
  endpoint/namespace;
- preserving the system's external network boundary.

The API Gateway must not contain Front Desk, KMS, FOSS, or WKMS business
logic.

## WebSocket Ownership

Real-time business events are owned by the backend that owns the associated
business state.

Each subsystem backend may maintain its own:

`src/realtime/`

module containing its Socket.IO/NestJS gateway and realtime event mapping.

The Front Desk backend therefore owns realtime delivery for Front Desk UI
concerns such as:

- room-status updates;
- new escalation notifications;
- escalation badge changes;
- relevant check-in/check-out dashboard refresh events.

WKMS owns realtime task-queue updates for worker clients.

KMS owns realtime kitchen-order state updates.

FOSS owns realtime guest order/service-request tracking updates.

MAD owns realtime manager-dashboard updates.

External WSS traffic is routed through the API Gateway.

## Central WebSocket Server Decision

A separate business-logic WebSocket microservice/repository will not be
introduced during the current implementation.

Instead:

- subsystem backends own their realtime namespaces/events;
- the API Gateway provides the external routing boundary;
- realtime state remains derived from authoritative backend state.

This differs from the original SDS wording describing one standalone
WebSocket server container.

The final SDS must be updated to reflect the implemented distributed
subsystem-gateway model if this ADR is approved.

A standalone WebSocket service may be introduced later only through a separate
architecture decision if operational scale requires it.

## RabbitMQ Ownership Model

RabbitMQ follows producer-owned events and consumer-owned queues.

### Producer Responsibilities

The subsystem that owns the business event is responsible for:

- defining the meaning of the event;
- publishing only after the related authoritative state transition succeeds;
- setting the required event envelope;
- maintaining backward-compatible contracts where possible;
- avoiding unnecessary PII in event payloads.

### Consumer Responsibilities

Each consuming subsystem is responsible for:

- its own queue;
- its own consumer handler;
- its own dead-letter handling;
- idempotent processing where duplicate delivery is possible;
- retry/failure behavior;
- consumer-side logging and monitoring.

## Event Envelope

RabbitMQ messages must follow the project communication format containing:

- `eventType`
- `sourceService`
- `timestamp`
- `correlationId`
- `payload`

`timestamp` must use ISO 8601 UTC format.

`correlationId` must use a UUID.

RabbitMQ event payloads must avoid guest PII where opaque identifiers can be
used.

## Durable Messaging

Production RabbitMQ exchanges and queues used for required business events must
be durable.

Consumer services must use dead-letter handling for messages that cannot be
processed successfully according to the agreed infrastructure standard.

RabbitMQ is responsible for reliable asynchronous delivery.

It does not replace application-level idempotency and transaction safety.

## FDS Event Responsibilities

The following Front Desk event responsibilities are frozen by this ADR.

### WKMS -> FDS: Task Escalation

WKMS owns task state and escalation detection.

When a task remains unclaimed beyond the approved escalation threshold, WKMS
is responsible for:

1. updating its authoritative task state as required;
2. publishing a task-escalation event through RabbitMQ.

FDS is responsible for:

1. consuming the escalation event;
2. resolving any additional display data through approved contracts where
   needed;
3. updating its local application/realtime state;
4. pushing the escalation notification to connected Front Desk clients via
   the FDS realtime gateway.

Flow:

```text
WKMS
  |
  | task escalated
  v
RabbitMQ
  |
  v
FDS consumer
  |
  v
FDS realtime gateway
  |
  v
Front Desk browser