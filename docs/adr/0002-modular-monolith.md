# 0002: Modular monolith

## Status

Accepted

## Date

2026-10-01

## Context

The backend needs clear separation between concerns such as accounts, households, tasks, and notifications. The team is two people, and operational overhead matters more than independent scaling, and everything runs on a single free VM.

## Options considered

- Microservices: independent deployment and scaling, but heavy operational and cognitive cost for a two-person project.
- Single unstructured monolith: simplest to start, but boundaries erode over time.
- Modular monolith: one deployable split into modules with enforced boundaries.

## Decision

Build one Spring Boot deployable split into these modules: accounts, households, tasks, profiles, scheduling, notifications, score, and shared. Module boundaries are enforced rather than conventional.

## Consequences

- One deployment, one database, simple local development and operations on one VM.
- Modules can be extracted into services later if a real need appears.
- How boundaries are enforced is decided in HUSH-126 and will be recorded in a later ADR.
