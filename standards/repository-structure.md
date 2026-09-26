# Forever Hotel Repository Structure Standard

## Purpose

This document defines the standard repository structure used across the
Forever Hotel development project.

The objective is to ensure that subsystem repositories remain consistent,
easy to understand, easy to maintain, and aligned with the agreed
subsystem-oriented repository architecture.

This standard supports ADR-001:
`Subsystem-Oriented Repository Architecture`.

## Scope

This standard applies to the following subsystem repositories:

- `hotel-website`
- `manager-dashboard`
- `front-desk`
- `food-ordering`
- `kitchen-management`
- `worker-management`

It also defines the purpose of the shared repositories:

- `api-gateway`
- `infra`
- `tests`
- `documents`

## Standard Subsystem Repository Structure

Where a subsystem contains both frontend and backend implementation,
the following structure should be used:

```text
subsystem-repository/
├── .github/
│   └── workflows/
├── frontend/
├── backend/
├── README.md
├── CHANGELOG.md
├── .gitignore
└── .env.example