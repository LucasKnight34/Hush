# 0005: Hybrid structure inside modules

## Status

Accepted

## Date

2026-10-02

## Context

ADR-0002 and ADR-0004 fix the module boundaries. HUSH-127 asked how each module is structured inside: layered (controller, service, repository), ports and adapters, or a pragmatic hybrid. The pure calculators in HUSH-95 (schedule) and HUSH-109 (reminder planner) must be testable without Spring or a database, while CRUD-heavy modules such as profiles should not get extra ceremony.

The spike prototyped the scheduling module in the hybrid layout on Spring Boot 4.1.1, Spring Modulith 2.1.1, Spring Data JPA, and PostgreSQL 16.

## Options considered

- Layered everywhere: controller, service, repository, with JPA entities as the model. Fewest classes, but domain logic ends up coupled to JPA and Spring, so the pure calculators would not be pure.
- Ports and adapters everywhere: strongest isolation, but every module gets domain models, ports, adapters, and mappers, roughly doubling the class count for CRUD-only modules.
- Hybrid, chosen per module: a pure `domain` package plus Spring and JPA adapters where a module has real logic, and plain layered code where it does not.

## Decision

Use the hybrid, decided per module.

**Rich modules (scheduling, the reminder planner in notifications, score)** get:
- `com.hush.<module>`: the public API only (interfaces, view records, events). Under Spring Modulith the module base package is the API.
- `domain`: pure Java records, sealed interfaces, and calculators. No Spring, no JPA. An ArchUnit rule enforces this.
- `infrastructure`: JPA entities, Spring Data repositories, and the adapter that maps between entity and domain record.
- `internal`: application services and the ports they use.
- `web`: controllers and request and response types.

**CRUD-style modules (profiles, accounts, households, and tasks)** use plain layers inside the module: `web` and `internal` (service, repository, entity). The JPA entity is the model. Validation lives in the entity's constructor or factory method. There is no separate domain record and no mapper.

**Entities versus domain records:** keep a separate domain record only where the domain shape differs from the table shape. In scheduling, `TriggerSpec` is a sealed interface stored as a type column plus parameters, so a mapper is worth it. Everywhere else the entity is the model.

**Package naming:** no `api` sub-package. Under Modulith a sub-package is internal, so `scheduling.api` fails verification unless it is annotated with `@NamedInterface`. Keep the public API in the base package instead.

## Consequences

- Pure calculators run as plain unit tests. In the prototype, `ScheduleCalculator` tests ran in 0.06 seconds with no Spring context.
- A Spring annotation added to a domain class fails the ArchUnit rule with the offending class named.
- Other modules cannot see domain, infrastructure, or internal types. In the prototype, `notifications` could only use `SchedulingApi` and `ScheduleView` from the base package.
- The scheduling slice has 11 types: 4 in domain, 3 in infrastructure, 2 in internal, 2 in the base package. A CRUD module should have about 4 to 5. These counts come from the prototype and an estimate, not from measurement across real modules.
- Each module chooses its weight when it is built, so a CRUD module that grows real logic can adopt `domain` later by moving classes.
- Not verified in the spike: controllers and the `web` package, Gradle builds, and `@ApplicationModuleTest` slices with JPA.
