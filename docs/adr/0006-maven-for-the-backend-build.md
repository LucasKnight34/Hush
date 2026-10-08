# 0006: Maven for the backend build

## Status

Accepted

## Date

2026-10-08

## Context

HUSH-54, HUSH-57, and HUSH-58 assumed Gradle with the Kotlin DSL, but the build tool was never decided. The HUSH-126 and HUSH-127 spike prototypes were built with Maven, and ADR-0005 lists "Gradle builds" as not verified. HUSH-249 compared the two on the stack HUSH will actually use: Spring Boot 4.1.1, Spring Modulith 2.1.1, Java 21, Testcontainers with Postgres 16, Spotless, and GitHub Actions on the free tier.

Both tools are free and open source, so cost does not separate them (docs/cost-plan.md needs no change).

## Options considered

- **Maven 3.9:** The Spring Boot parent manages dependency versions. Surefire and failsafe separate unit tests from integration tests with no extra code. Verbose XML. The two existing spikes already run on it.
- **Gradle 9.8 with the Kotlin DSL:** A shorter build file (about 40 lines against 55 in the prototype), incremental builds, and a build cache that pays off in large multi-project builds. Needs a custom task or test suite to split unit and integration tests, and has more version churn between Gradle, the Spring Boot plugin, and plugins such as Spotless.

## Decision

Use Maven for the backend.

The prototype was identical under both tools: two modules, a Spring Modulith `verify()` test, a Spotless check with Google Java Format, and one Testcontainers test against Postgres 16. Both passed, and both failed a deliberately misformatted file in the Spotless check. A warm clean build took about 7 seconds with Maven and about 9 seconds with Gradle (without the Gradle daemon), so speed does not decide this. The same Maven prototype also passed in GitHub Actions on `ubuntu-latest` (Temurin 21, `actions/setup-java` Maven cache) in 48 seconds, with the Postgres container running on the hosted runner and no extra setup.

The reasons for Maven:
- ADR-0004 and ADR-0005 were proven on Maven, so choosing it means no migration and no open "not verified on Gradle" caveat.
- HUSH-214 asks for unit and integration tests to be separated, which Maven does out of the box.
- HUSH will be one application module with one build file, so Gradle's incremental and caching strengths matter little.

Conventions:
- Spring Boot's `spring-boot-starter-parent` is the parent POM, and `spring-modulith-bom` is imported in `dependencyManagement`.
- Unit tests run in surefire (`*Tests`, `*Test`) and integration tests in failsafe (`*IT`), both through `mvn verify`.
- Spotless runs `check` in the `verify` phase. `mvn spotless:apply` fixes formatting locally.
- Commit the Maven wrapper (`mvnw`) so contributors and CI need no separate Maven install.

## Consequences

- HUSH-54, HUSH-57, and HUSH-58 change from Gradle to Maven: `./gradlew check build` becomes `./mvnw verify`, and the Spotless setup uses `spotless-maven-plugin`.
- The HUSH-126 and HUSH-127 prototypes need no change.
- The Spotless Maven plugin version is pinned in the POM so formatting results stay stable.
- The XML is more verbose than Gradle's Kotlin DSL. Revisit only if the backend is split into several deployable modules, where Gradle's multi-project support becomes worth the migration.
- `actions/setup-java@v4` and `actions/checkout@v4` showed deprecation warnings in the run, so HUSH-58 should use the current major versions (setup-java v5).
- Not verified: the Docker image build for HUSH-62 (Spring Boot buildpacks on ARM64), and JaCoCo coverage for HUSH-218. Neither was part of this prototype.
- Not measured: how often each tool appears in job postings. No reliable data source was used, so this did not influence the decision.
