# Known Risks

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu

These are the risks the team identified, with the planned mitigation for each. Where a risk names a business rule, the US-IDs in parentheses name the user stories whose acceptance criteria test that rule.

| Risk | Mitigation |
| --- | --- |
| Someone becomes unavailable. | use secondary owners for every area, distribute issues fairly each milestone, and follow the team's escalation steps. |
| Scope creep. | any new feature idea goes through the full team before being added; out-of-scope items are logged but deferred. |
| Integration issues between frontend and backend. | agree on API contracts early and document them in the design doc before implementation begins. |
| The stack has a learning curve. Some members may be new to React, TypeScript, FastAPI, or PostgreSQL. | build a thin end-to-end slice early by R1, pair on hard parts, and use lecture material and office hours. |
| Core business-rule bugs sneak in. The highest-risk rules are over-claim prevention (US-08, US-10), status transitions (US-10, US-11, US-26, US-16, US-17), and no cancellation after pickup (US-11, US-26). | write automated tests for these rules early, cover edge and negative paths, and require review on any pull request that touches them. |
| Setup drifts across machines. | everyone sets up their environment in week 1, we keep setup steps and seeded demo data in the setup guide, and CI catches breakages early. |
| Deadlines get tight. | track milestones on the board, review progress in the weekly meeting, and protect must-have work before nice-to-have work. |
