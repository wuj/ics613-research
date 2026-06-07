# Known Risks

**Team 4** | Surplus (Local Produce Exchange)
ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026
Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu

These are the risks the team identified, with the planned mitigation for each. Where a risk names a business rule, the linked user stories are the ones whose acceptance criteria test that rule.

| Risk | Mitigation |
| --- | --- |
| Someone becomes unavailable. | use secondary owners for every area, distribute issues fairly each milestone, and follow the team's escalation steps. |
| Scope creep. | any new feature idea goes through the full team before being added; out-of-scope items are logged but deferred. |
| Integration issues between frontend and backend. | agree on API contracts early and document them in the design doc before implementation begins. |
| The stack has a learning curve. Some members may be new to React, TypeScript, FastAPI, or PostgreSQL. | build a thin end-to-end slice early by R1, pair on hard parts, and use lecture material and office hours. |
| Core business-rule bugs sneak in. The highest-risk rules are over-claim prevention ([US-08](01-user-stories.md#us-08-submit-a-request-for-an-item), [US-10](01-user-stories.md#us-10-approve-or-deny-a-pending-request)), status transitions ([US-10](01-user-stories.md#us-10-approve-or-deny-a-pending-request), [US-11](01-user-stories.md#us-11-withdraw-a-queued-request), [US-26](01-user-stories.md#us-26-cancel-an-approved-request), [US-16](01-user-stories.md#us-16-confirm-pickup), [US-17](01-user-stories.md#us-17-complete-an-exchange)), and no cancellation after pickup ([US-11](01-user-stories.md#us-11-withdraw-a-queued-request), [US-26](01-user-stories.md#us-26-cancel-an-approved-request)). | write automated tests for these rules early, cover edge and negative paths, and require review on any pull request that touches them. |
| Setup drifts across machines. | everyone sets up their environment in week 1, we keep setup steps and seeded demo data in the README, and CI catches breakages early. |
| Deadlines get tight. | track milestones on the board, review progress in the weekly meeting, and protect must-have work before nice-to-have work. |
