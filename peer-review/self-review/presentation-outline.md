# Requirements Review Presentation Outline

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu\
**Reviewed team:** Team 4 | Local Produce Exchange, "Surplus" (self-review)

Total time: 10 minutes.

## Slide 1: Title and reviewed team
- Introduce the review: a self-review of Team 4's own Requirements Review Packet for the Local Produce Exchange project, "Surplus".
- One sentence on why: rehearsing the cross-team review process on our own packet before reviewing, and being reviewed by, another team.
- Time: 0:30

## Slide 2: What we reviewed and how
- Six files: a README cover page plus the user stories (30 stories, US-01 to US-30), acceptance criteria (the pass-or-fail checks that say when a story is done) for every story, user roles and personas, open questions and assumptions (OQA-01 to OQA-09), and known risks (seven rows).
- All five required packet items are present; the optional domain model is not included.
- Method: seven passes covering story quality, criteria testability, role coverage, edge cases, cross-story consistency, the open questions, and the risks.
- Time: 1:00

## Slide 3: Summary impression
- The packet is consistent in format: every story has an actor, action, benefit, priority, source use case, and milestone, and every story has Given/When/Then criteria.
- The defects found are concentrated, not spread out: two garbled sentences inside workflow rules, one lifecycle contradiction, one review-visibility contradiction, and a few unbounded or undecided areas.
- Nothing found blocks the design phase; everything found is fixable with wording changes and a handful of decisions.
- Time: 1:00

## Slide 4: Strengths
- F-01: uniform story format, 30 stories at the top of the assignment's 25 to 30 range.
- F-02: criteria with concrete numbers (10 units, request 3, reject 12, reject 0) and empty-state coverage.
- F-03: permissions written as deny scenarios in nearly every story, matching the instructor's guidance.
- F-04: roles match the assignment baseline, with a role-to-story coverage table.
- F-05: open questions and risks name the exact story IDs they affect.
- Time: 1:30

## Slide 5: Ambiguities
- F-06 (high): US-26 calls PICKED_UP a terminal status; US-17 and the project description move PICKED_UP to COMPLETED, and COMPLETED is missing from the cancel-rejection list.
- F-07 (high): US-30 shows any member another member's review history while US-19 denies non-participants access to reviews; the two rules collide unless review history means a summary.
- F-08: "Restaurants the quantity back to the listing's quantity" needs to become a testable restore rule.
- F-09: submission checks units available, approval checks remaining quantity; which bounds a new request?
- F-11 and F-12: a garbled unsuspend outcome, and an allowed-to-invite rule no story manages.
- Time: 2:00

## Slide 6: Missing cases
- F-16: R1 stories browse, request, and message over listings that only an R2 story can create, and R1 criteria assert notifications that only R2 stories can show.
- F-17: suspension freezes the account but says nothing about the member's active listings or in-flight requests.
- F-18: pickup windows are required but nothing covers a window that passes; an approved, never-picked-up request holds quantity forever.
- F-19: photos can be uploaded and managed but no story displays them to anyone.
- Time: 2:00

## Slide 7: Scope risks
- F-21: "a basic report about system activity" has no named reports, columns, or filters; pick two or three concrete reports.
- F-22: photo upload needs storage and serving decisions; OQA-07 covers file types and size, but media is absent from the risks list.
- Time: 1:00

## Slide 8: Questions for the team
- Which statuses must reject a cancel attempt under US-26, and is PICKED_UP meant to appear in that list? (F-06)
- Does review history on the public profile mean full review text or a summary rating? (F-07)
- At submission time, is the cap the original quantity available or the current remaining quantity? (F-09)
- How will the R1 stories run without listing creation in R1? (F-16)
- When an account is suspended, what happens to its listings and in-flight requests? (F-17)
- Time: 0:45

## Slide 9: Closing
- The packet is review-ready in structure; the fixes are wording repairs and a short list of decisions to confirm before design.
- Time: 0:15
