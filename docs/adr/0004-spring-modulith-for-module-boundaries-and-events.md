# 0004: Spring Modulith for module boundaries and events

## Status

Accepted

## Date

2026-10-02

## Context

ADR-0002 chose a modular monolith and left open how module boundaries are enforced. HUSH-126 asked whether to use Spring Modulith, ArchUnit, or Gradle multi-project modules, and whether Spring Modulith's event publication registry is the right way to deliver domain events between modules (HUSH-103).

The spike prototyped two modules (tasks, scheduling) on Spring Boot 4.1.1 and Spring Modulith 2.1.1 with PostgreSQL 16 and Java 21. The tasks module publishes a TaskCompleted event and the scheduling module listens to it.

## Options considered

- Spring Modulith: verifies module structure in a test, generates C4 and PlantUML diagrams, provides module-level test slices, and an event publication registry.
- ArchUnit alone: flexible rules, but each rule is hand-written and there is no module concept, no diagrams, and no event support.
- Gradle multi-project modules: strongest compile-time enforcement, but it adds build overhead for eight small modules and offers no events, diagrams, or test slices. Assessed from documentation and experience, not prototyped.

## Decision

Use Spring Modulith as the primary tool:
- A test calls `ApplicationModules.verify()` on every build. A module reaching into another module's internal sub-package fails with a message naming both modules and the offending call.
- A module's base package is its public API. Sub-packages such as `internal` are not exposed to other modules.
- Use `@ApplicationModuleListener` and the JDBC event publication registry for cross-module domain events (HUSH-103).
- Generate module diagrams with the Documenter for the architecture README.
- Use `@ApplicationModuleTest` for module-level integration tests, with the `Scenario` API for event-driven flows.

Keep ArchUnit as a complement for rules Modulith does not express (naming, layering inside a module, no repository access from controllers). Modulith already depends on ArchUnit core, but `archunit-junit5` must be added explicitly.

Do not use Gradle multi-project modules now. Revisit if a module is ever extracted into its own deployable.

## Consequences

- Boundary violations fail the build with a clear message. In the prototype, Modulith verification and a hand-written ArchUnit rule both caught the same violation.
- Events are stored in the `event_publication` table in the publisher's transaction. In the prototype, a JVM killed inside the listener left the publication incomplete, and the next start with `spring.modulith.events.republish-outstanding-events-on-restart=true` re-delivered it. With that flag off (the default), the event stayed incomplete and was never delivered.
- Delivery is at least once, so listeners must be idempotent. Replay on restart is not enabled by default and must be turned on deliberately.
- With two API containers sharing one database (ADR-0003), a restarting instance could resubmit an event another instance is still processing. This was not tested in the spike. Idempotent listeners are the mitigation, and HUSH-103 should test it.
- Completed rows accumulate in `event_publication`. HUSH-103 should plan cleanup and manage the table with Flyway rather than schema auto-initialization.
- Module integration tests load only the module under test and its declared dependencies, so they stay fast.
- Boundary rules live in test code, not the compiler. A module can still be bypassed if the verification test is not run.
