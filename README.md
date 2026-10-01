# HUSH

**H**andling **U**nseen **S**tuff at **H**ome. Or, if you prefer the cheeky version: **H**elping **U**s **S**top **H**ollering.

HUSH is an iOS app for couples to share the infrequent, invisible household work that nobody remembers until it is overdue: pet grooming based on breed and activity, HVAC filters, car maintenance by mileage, document renewals, gift lead times, and seasonal home prep. It is not a daily chore tracker. It is a personal project for Lucas and Sarah, and a backend portfolio piece.

## Tech stack

- Java 21, Spring Boot (modular monolith)
- PostgreSQL with Flyway migrations
- SwiftUI (iOS)
- Docker
- GitHub Actions
- AWS: ECS Fargate, RDS, SES

## Repo map

| Path | Contents |
| --- | --- |
| `backend/` | Spring Boot modular monolith |
| `ios/` | SwiftUI app |
| `dataset/` | Curated task dataset and seed files |
| `docs/adr/` | Architecture decision records |
| `docs/product/` | Product brief, personas, story map |
| `docs/design/` | Exported flows and wireframes |
| `docs/definition-of-done.md` | Definition of Ready and Definition of Done |
| `.github/workflows/` | CI workflows |
| `CONTRIBUTING.md` | Branch, commit, and PR conventions |

## Architecture

_Architecture diagram placeholder. To be added once the first backend modules exist._

## Project status

Sprint 0: setup and planning. No application code yet.

## Planning

Jira is the source of truth: https://lucasknight336-1790873410738.atlassian.net/jira/software/projects/HUSH

## License

MIT, see [LICENSE](LICENSE).
