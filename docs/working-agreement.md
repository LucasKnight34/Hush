# HUSH Working Agreement

How Sarah and Lucas build HUSH together. Agreed at the start of Sprint 0 and revisited at every retro.

## Roles

**Sarah, Product Owner**
- Owns the backlog order and decides what matters most each sprint
- Writes and accepts the definition of "done right" for user-facing stories
- Owns the task dataset, copy, and design direction
- Moves user-facing tickets from Sarah's Acceptance to Done

**Lucas, Developer and Scrum Master**
- Builds the backend and iOS app, and estimates technical work
- Runs the sprint sessions and keeps the board accurate
- Flags risks, blockers, and scope creep early
- Writes decision records and spike findings

## Sprint cadence

- Sprints are two weeks, Sunday 8:00 PM to Sunday 8:00 PM (America/Denver)
- Sprint 0 is a short setup sprint: Oct 1 to Oct 11, 2026. It does not count toward velocity
- Sprint 1 starts Sunday, Oct 11 after the sprint session and ends Sunday, Oct 25
- Only the current sprint and the next one exist in Jira at any time

## Ceremonies

| Ceremony | When | Length | What happens |
|---|---|---|---|
| Sprint session | Every other Sunday, end of sprint | About 75 minutes | Review (demo on our phones), retro, close the sprint, plan the next one |
| Refinement | The Sunday between sprint sessions | 20 minutes | Get next sprint's top tickets to Ready |
| Async standup | Tuesday and Friday | 2 minutes | A text with three lines: done, next, blocked |

**Retro questions:** What helped? What got in the way? What do we try next sprint? We pick one change, not five.

## Board workflow

| Column | Meaning | Who moves it out |
|---|---|---|
| To Do | Planned for this sprint, not started | The person who picks it up |
| In Progress | Actively being worked | The person working it |
| In Review | Code in a pull request, or a design or doc waiting on the other person | Reviewer |
| Sarah's Acceptance | User-facing story deployed to dev and ready for Sarah to try | Sarah |
| Done | Meets the Definition of Done | Nobody |

Technical tasks with no user-facing behavior skip Sarah's Acceptance and go from In Review to Done.

## Planning rules

- Story points use Fibonacci: 1, 2, 3, 5, 8. Anything that would be 13 gets split
- Lucas's capacity is about 25 hours per sprint. Until we have three sprints of history, he commits 15 to 20 points
- Sarah sets her own capacity at each sprint session. Her content and design work is tracked by label and not counted against Lucas's points
- Only tickets that meet the Definition of Ready go into a sprint
- A spike comes before any ticket that introduces a new library, external service, AWS resource, or architecture choice

## Mid-sprint changes

- New ideas go to the backlog, not into the current sprint
- If something truly can't wait, Sarah swaps it in and takes out work of the same size
- Bugs that block the other person's work jump the line; everything else waits for planning

## Where things live

- **Jira (HUSH project):** all planned work, bugs, and sprint history
- **Repo docs/adr:** architecture decision records
- **Confluence:** spike findings, linked from each spike ticket
- **Repo docs/definition-of-done.md:** Definition of Ready and Definition of Done

## Ground rules

- HUSH talk stays in standups and sprint sessions, not at dinner
- Either of us can say "not this sprint" without it being a big deal
- Feedback on the app is about the app. If a reminder feels naggy, that's a bug to fix, not a sign someone is failing
- If the project stops being fun or useful, we say so at retro and decide together whether to change course

## Agreement

Agreed by Sarah and Lucas on October 1, 2026. Revisit at every retro; change it whenever it stops working.
