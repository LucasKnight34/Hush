# ADR-0003: Host HUSH on an Oracle Cloud Always Free VM with Docker Compose

- **Status:** Accepted
- **Date:** October 2026
- **Jira:** HUSH-53
- **Related:** docs/cost-plan.md (full cost inventory and upgrade path)

## Context

HUSH is a two-person app and a portfolio project that must cost $0 to build and run through beta. The only accepted spend is the existing Apple Developer Program membership.

The backend must be always on: the reminder dispatcher sends pushes at scheduled times, so hosting that sleeps on idle traffic would miss reminders. The dispatcher also needs at least two app instances against one PostgreSQL database to prove it never double-sends.

Constraints discovered during the spike:
- AWS App Runner closed to new customers on April 30, 2026.
- New AWS accounts get temporary credits, not a 12-month free tier.
- Render, Koyeb, and Fly.io either sleep on idle traffic or no longer offer free compute.
- Oracle Cloud's Always Free ARM allowance is 2 OCPUs and 12 GB of RAM (since June 15, 2026).

## Options considered

1. **AWS ECS on Fargate with an ALB and RDS:** about $58 to $76 a month. Highest portfolio value.
2. **AWS ECS Express Mode:** same cost as option 1, less setup.
3. **Lightsail containers and database:** about $30 to $45 a month.
4. **Single AWS EC2 instance:** about $17.50 a month.
5. **Render or other free PaaS tiers:** $0, but they sleep or expire.
6. **Google Cloud e2-micro:** $0, but 1 GB of RAM is too small.
7. **Oracle Cloud Always Free ARM VM with Docker Compose:** $0, always on, plenty of capacity.

## Decision

Run HUSH on one Oracle Cloud Always Free VM.Standard.A1.Flex instance (ARM64, 2 OCPU, 12 GB) with Docker Compose:
- **Two API containers**, so the dispatcher runs as two competing instances
- **PostgreSQL** in a container with a persistent volume, not exposed outside the VM
- **Caddy** for automatic HTTPS and load balancing across both API containers

Supporting choices:
- The Oracle account is converted to **Pay As You Go** to avoid idle reclamation, with a $1 budget alert. Spend stays $0 inside Always Free limits.
- **GitHub Actions** builds ARM64 images, publishes them to GHCR, and deploys over SSH with a rolling Compose update.
- **Secrets** live in GitHub environment secrets and are written to a permission-restricted env file on the VM.
- **Nightly pg_dump** backups go to Oracle Object Storage (Always Free).
- **A DuckDNS subdomain** serves during development. A real domain is bought at public launch.
- The VM, network, and storage are defined in **infrastructure as code** (tool chosen in HUSH-129).

## Consequences

**Positive**
- $0 a month through development and beta.
- Generous headroom: about 12 GB of RAM for two JVMs, Postgres, Caddy, and optional self-hosted monitoring.
- Portable: the same images and Compose file run on any VPS or cloud, so moving to a paid tier is a deploy change, not a rewrite.
- A clear, cost-driven architecture story for interviews.

**Negative**
- One VM is a single point of failure. Mitigated by backups, documented rebuild steps, and an uptime monitor.
- We operate Postgres ourselves: upgrades, backups, and restore testing.
- Both API containers share one host, so the dispatcher concurrency proof is across processes, not machines. The free allowance can be split into two 1-OCPU VMs if a cross-host proof is wanted (HUSH-125).
- No hands-on ECS, ALB, or IAM experience from this project.
- Oracle has changed Always Free limits before without notice.

**Revisit when**
- Oracle reduces or removes the Always Free allowance: move to a small paid VPS (about $5 a month).
- HUSH has real users beyond the beta and needs higher availability: see the upgrade path in docs/cost-plan.md.
- An AWS portfolio demo is wanted: add an optional, on-demand ECS environment funded by AWS credits, torn down after use.
