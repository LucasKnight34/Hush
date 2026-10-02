# Project rules for Claude Code

- Read the Jira ticket (project HUSH) before starting work, and treat its acceptance criteria as the definition of done.
- Branch and commit naming follow CONTRIBUTING.md: branches like `HUSH-123-short-description`, commits like `HUSH-123: Add reminder table`.
- Never add AI attribution to commits or PRs: no Co-Authored-By trailers, no "Generated with" lines.
- Never use em dashes in any file, commit, or message.
- Do no work outside the ticket's scope.
- New technical choices need a spike ticket first.
- Decisions go in `docs/adr`.
- Development must cost $0. Do not add or reference AWS or any paid service. Check `docs/cost-plan.md` before adding any service and update it when one is added.
- Spike findings go to the HUSH Confluence space (key HUSH) as a page under "Spikes", titled "Spike HUSH-123: Short title". Link the page in a comment on the spike ticket. Decisions stay in `docs/adr`.
