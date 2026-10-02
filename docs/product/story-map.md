# Story map and MVP line

**Status:** Draft. This is a proposal from Lucas's side. Sarah, as Product Owner, approves or changes the line (HUSH-32). Nothing in Jira has been changed yet.

The map follows one user journey across the top: onboarding, setup, upcoming, complete, reminders, household, score. Under each step the capabilities are split by a line into **MVP** (needed for the two of us to use HUSH every week), **stretch** (worth doing if time allows), and **later** (after the MVP is working).

The product brief's success criterion is that both of us use HUSH every week for a month. The MVP is whatever that takes, and no more.

## Onboarding

| Tier | Capabilities |
|---|---|
| MVP | Sign in with Apple (HUSH-66, 159). Home questionnaire (174). Pet and vehicle quick setup (175). Starter schedule from the dataset (176), handling "not sure when last done" (177), starter limit with safety tasks always included (178). Invite step (179) |
| Stretch | None |
| Later | None |

## Setup

| Tier | Capabilities |
|---|---|
| MVP | Create, view, and edit tasks (78, 166). Categories and importance (79). Assign a task and set effort (80). Household details on a task (81). Pet, vehicle, and home profiles and important dates (87 to 90, 170 to 172). Trigger types: fixed schedule (97), after completion (98), anchored with lead time (99), mileage (100), pet-derived (101). Choose a trigger type and preview the schedule (167, 104). Settings, notification preferences, time zone and quiet hours (69, 173) |
| Stretch | Weather and condition triggers, including first-freeze (epic E11, HUSH-138 to 144) |
| Later | Seasonal and life-stage task packs (242) |

## Upcoming

| Tier | Capabilities |
|---|---|
| MVP | Upcoming view grouped by month and category (164). Task detail with how-to and stored details (165). Next occurrence generated on completion (102) |
| Stretch | Home screen widget (136, 151) |
| Later | Calendar sync (244). Household memory search (245) |

## Complete

| Tier | Capabilities |
|---|---|
| MVP | Mark done and record history (82). Logbook with filters (83, 168). Delete a task without losing history (85). Done and Snooze from the lock screen (135, 145, 146, 147) |
| Stretch | None |
| Later | Receipt and photo storage on completions (246) |

## Reminders

| Tier | Capabilities |
|---|---|
| MVP | Reminder plan: heads-up, day-of, overdue (109). Reminder table with UTC send time (110). Quiet hours and time zone adjustment (111). Dispatcher with SKIP LOCKED and ShedLock (112, 113). Idempotency, retry, and dead letters (114 to 116). Notification channel and APNs (117 to 120, 160). Replan on completion (124). No duplicate sends across instances (125) |
| Stretch | Weekly email digest (121). In-app notification inbox (122) |
| Later | Message queue with transactional outbox (247) |

## Household

| Tier | Capabilities |
|---|---|
| MVP | Create a household, invite link, join (71 to 73). Household-scoped authorization and isolation tests (74, 76, 206). "I'll take it" reassignment (148). Members and invite screen (169). Category default owners (see the personas) |
| Stretch | Leave a household or remove a member (75). Handoff state machine and handoff notifications (137, 149, 150) |
| Later | Android or web access (243) |

## Score

| Tier | Capabilities |
|---|---|
| MVP | Score formula and decision record (180, 182, 189). Category vitals (183). Event-driven recompute (184). Cold-start grace periods (185). Score screen on iOS (190) |
| Stretch | House visuals tied to vitals and animation (36, 41, 181, 191). Anti-gaming rules, daily snapshots, and trend endpoint (186 to 188). Gamification: seasonal readiness goals, early-bird credit, milestones, co-op balance view, celebrations, simple mode (192 to 197) |
| Later | Cosmetic unlocks (240). Sunday recap (241) |

## Foundation work (not on the journey, needed for the MVP)

- Repo, CI, local dev, Flyway, linting, and the $0 Oracle environments (epic E4, tickets 53 to 65).
- Architecture decisions and spikes (126 to 131).
- Auth and tokens (66 to 68), task and profile model (77, 86, 91, 92), recurrence engine and its time-zone and DST test suites (93 to 107).
- iOS foundation: project structure, networking, Keychain, navigation (epic E13).
- Design: palette, wireframes, mockups, and Sarah's sign-off (epic E2). The dataset and content (epic E3).
- Testing infrastructure (epic E20), beta with both of us and the 30-day dogfooding period (epic E21).
- Security basics: input validation and secrets audit (208, 209).
- Observability basics: metrics, uptime check, structured logs (198, 200, 202, 203). The dashboard, alerts, and k6 load test are stretch (201, 204, 205).

## After the MVP (later)

- App Store release (epic E22), including account deletion, privacy policy, and privacy labels (70, 210, 211, 213). These are required for the App Store but not for two people on TestFlight.
- Portfolio packaging (epic E23). It can run alongside, but it is not part of the product MVP.
- Later ideas (epic E24).

## Epic scope against the line

Points are the Jira story points of tickets that have an estimate. Spikes are counted under the epic they feed, so a few assignments are approximate. Epics E22 to E24 are mostly unestimated.

| Epic | MVP | Stretch | Later |
|---|---|---|---|
| E1 Discovery and Definition | 12 | 0 | 0 |
| E2 UX and Visual Design | 29 | 8 | 0 |
| E3 Task Dataset and Content | 24 | 0 | 0 |
| E4 Infrastructure and DevOps | 55 | 0 | 0 |
| E5 Auth and Accounts | 21 | 0 | 3 |
| E6 Household and Sharing | 17 | 3 | 0 |
| E7 Task Model and Household Memory | 32 | 0 | 0 |
| E8 Profiles | 25 | 0 | 0 |
| E9 Recurrence and Scheduling Engine | 58 | 0 | 0 |
| E10 Notification Platform | 61 | 10 | 0 |
| E11 Weather and Condition Triggers | 0 | 25 | 0 |
| E12 Quick Actions and Handoff | 17 | 17 | 0 |
| E13 iOS App Foundation | 34 | 0 | 0 |
| E14 iOS Core Screens | 36 | 0 | 0 |
| E15 Onboarding and Smart Defaults | 28 | 0 | 0 |
| E16 House Health Score | 23 | 13 | 0 |
| E17 Gamification | 0 | 21 | 0 |
| E18 Observability and Reliability | 9 | 10 | 0 |
| E19 Security and Privacy | 10 | 8 | 6 |
| E20 Testing and Quality Infrastructure | 14 | 0 | 0 |
| E21 Beta and Dogfooding | 15 | 0 | 0 |
| E22 App Store Release | 0 | 0 | 6 |
| E23 Portfolio Packaging | 0 | 0 | 0 |
| E24 Later Ideas | 0 | 0 | 0 |
| **Total (estimated tickets)** | **520** | **115** | **15** |

## Capacity check

The MVP column totals about 520 points, and about 450 of those are Lucas's engineering work once Sarah's design, content, and discovery work is set aside (the working agreement tracks hers separately). At 15 to 20 points per sprint, that is roughly 25 sprints, or about a year. The line above is already trimmed, and it is still long. Options if that is too long:

- Cut further: for example observability basics, the coverage reporting in CI, or one trigger type such as mileage (8 points) until after a first release.
- Accept the timeline and treat the first few months as a smaller milestone, such as tasks, one trigger type, and push reminders for the two of us.
- Revisit capacity after three sprints of real velocity, as the working agreement already says.

## Jira changes this proposal needs (not done yet)

Jira's mvp label is on almost every ticket today. If the line is approved, these labels would change:

- **To stretch:** 75, 122, 136, 137, 149, 150, 151, 36, 41, 181, 186, 187, 188, 191, 192 to 197, 199, 201, 204, 205, 207, 212.
- **To later:** 70, 210, 211, 213, 226 to 231, and the portfolio tickets 232 to 237 and 239.
- Already marked stretch and unchanged: 121, 133, 134, 138 to 144, 238.

## Questions for Sarah

- Is this the right MVP line? What would you move across it?
- Is the score in the MVP only as a plain score and screen, with the house visuals and game elements held back?
- Are handoff, the widget, and the weather triggers fine as stretch?
- Is a year to the full MVP acceptable, or do we cut further?
