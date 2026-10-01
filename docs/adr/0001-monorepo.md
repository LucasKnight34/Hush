# 0001: Monorepo

## Status

Accepted

## Date

2026-10-01

## Context

HUSH has a Spring Boot backend, a SwiftUI iOS app, a curated task dataset, infrastructure code, and project documentation. Two people work on it part time, and the API and its client change together often.

## Options considered

- Monorepo: backend, iOS, dataset, and docs in one repository.
- Separate repositories per component: independent histories and CI, but changes that span an API and its client need coordinated PRs.

## Decision

Use a single repository. Backend, iOS, dataset, infra, and docs live together so one PR can change an API and its client, and reviewers see the whole system in one place.

## Consequences

- Cross-cutting changes are atomic and easy to review.
- Trade-off: CI needs separate paths per folder so a docs change does not run backend builds and an iOS change does not run backend tests. Path filters are set up in HUSH-58.
- One issue tracker and one set of contribution conventions for everything.
