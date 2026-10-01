# HUSH Cost Plan

Every service HUSH depends on, what it costs, the free option we use, and when we'd pay.

**Rule:** development and beta cost $0. The only accepted spend is the Apple Developer Program, already covered by the existing Lift Knight membership.

**Owner:** Lucas. **Review:** at the sprint review closest to the start of each quarter, and whenever a provider changes its free tier.

**Decision record:** docs/adr/0003-free-tier-hosting-on-oracle-cloud.md

## Inventory

| Need | Why HUSH needs it | Free choice | Free limits that matter | Paid fallback |
|---|---|---|---|---|
| Apple Developer Program | APNs push, Sign in with Apple, TestFlight, App Store | Existing membership (covers unlimited apps) | Must stay active | $99/year |
| Backend compute | API and the always-on reminder dispatcher | Oracle Cloud Always Free A1 ARM VM (2 OCPU, 12 GB) | Home region only; capacity not guaranteed; Pay As You Go conversion avoids idle reclamation | Small VPS, about $5/month |
| Database | Households, tasks, schedules, reminders | PostgreSQL container on the same VM | Shares VM disk (Always Free block storage is 200 GB total) | Managed Postgres, about $15 to $25/month |
| Load balancing and HTTPS | One public URL across two API containers, TLS | Caddy with Let's Encrypt | None at this scale | Included with any paid platform |
| Hostname | Stable URL for the iOS app, TLS certificates | DuckDNS free subdomain | No custom email or DKIM records | Real domain, about $10 to $12/year (buy at launch) |
| Secrets | APNs key, DB password, JWT signing keys | GitHub environment secrets, written to an env file on the VM | Manual rotation | A secrets manager |
| Container registry | Versioned images for deploys | GitHub Container Registry (public images) | Public repo and images | Free for private use within limits |
| CI/CD | Build, test, deploy | GitHub Actions | Free and unlimited for public repos | Free minutes cap for private repos |
| Backups | Recover from a lost VM or bad migration | Nightly pg_dump to Oracle Object Storage | 20 GB Always Free | Any object storage, cents/month |
| Push notifications | Lock-screen reminders | APNs | Free with Apple membership | None |
| Sign in | Account creation and login | Sign in with Apple | Free with Apple membership | None |
| Email digest | Weekly "coming up" email | Resend free tier | 3,000 emails/month, 100/day, 1 verified domain. Needs a real domain for real recipients | Resend Pro $20/month or SES about $0.10 per 1,000 |
| Weather | First-freeze and seasonal triggers | Open-Meteo | Free for non-commercial use only | Paid API plan if HUSH ever charges money |
| Monitoring and alerts | Dashboard, dispatcher health, dead-letter alerts | Grafana Cloud free tier, or Prometheus and Grafana self-hosted on the VM | Check current free-tier limits when E18 is planned | Grafana Cloud paid tiers |
| Uptime checks | Know when the API is down | UptimeRobot free plan | Check interval limits on free plan | Paid plan |
| Load testing | k6 dispatcher test | k6 open source | Run from a laptop or CI | Skip k6 Cloud |
| Planning | Jira and Confluence | Atlassian free plans | Up to 10 users | Paid per user |
| Design | Wireframes and mockups | Figma Starter, or Penpot | Figma Starter file limits | Figma paid seats |
| Privacy policy | Required by the App Store | GitHub Pages | Public site | None |
| Infrastructure as code | Reproducible VM, network, storage | Terraform or OpenTofu with the OCI provider, state in Oracle Object Storage | None | None |

**Development tooling:** Claude Code requires a paid Claude plan. That is an existing personal subscription, not an app cost.

## Costs that appear only at launch

| Item | When | Cost |
|---|---|---|
| Real domain name | Before App Store submission (needed for email, Universal Links, and a polished API URL) | About $10 to $12/year |
| Resend verified domain | Same time; turns on the email digest for real users | $0 on the free tier |

## Upgrade path

HUSH's load is small by design. A household generates roughly 100 reminders a month, so 10,000 households is about 1 million reminders a month, around one every three seconds. A 2-core VM handles that. Upgrades will be driven by reliability and operating effort, not load.

| Stage | Households | Setup | Monthly cost | Move up when |
|---|---|---|---|---|
| Build and beta | Up to about 100 | Oracle Always Free VM, Compose, DuckDNS | $0 | Ready for public launch |
| Public launch | Up to a few thousand | Same VM, plus a real domain, Resend for email, offsite backups, uptime monitor | About $1 (domain) | Downtime or restore risk is no longer acceptable, or Oracle limits change |
| Growing | Several thousand to tens of thousands | Small paid VPS (about $5/month), optionally managed Postgres | About $5 to $30 | Need redundancy, zero-downtime database maintenance, or less ops time |
| Serious scale | Tens of thousands and up | Managed containers and database (for example AWS ECS and RDS) | About $75 and up | n/a |

**Watch these free-tier ceilings:**
- **Resend:** 100 emails a day. With a weekly digest, that's roughly 700 users. Move up before that.
- **Open-Meteo:** free for non-commercial use only. Any paid version of HUSH needs a commercial plan.
- **Oracle Always Free:** already reduced once, in June 2026. Re-check at every review.
