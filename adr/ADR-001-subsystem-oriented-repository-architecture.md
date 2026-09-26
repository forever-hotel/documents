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