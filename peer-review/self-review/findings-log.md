# Findings Log

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu\
**Reviewed team:** Team 4 | Local Produce Exchange, "Surplus" (self-review)

Internal log of every finding from the self-review of Team 4's own outgoing packet, grouped by category, severity-descending within each category.

### F-01: Uniform story format with traceable metadata
- Category: Strength
- Severity: n/a
- Location: Team 4 - 01 - User Stories.pdf, all sections, pages 1 to 7
- Quote: "As a guest, I want to register for an account using a valid invite token so that I can join the trusted community." (US-01, page 1)
- Finding: Every story names an actor, an observable action, and a benefit, plus a priority, a source use case, and a milestone. The count of 30 stories sits at the top of the assignment's 25 to 30 range (ics613-research/requirements.md, Core Requirements section), and the stories read as distinct functionalities.
- Question: none

### F-02: Concrete, testable acceptance criteria with empty states
- Category: Strength
- Severity: n/a
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-08 pages 4 to 5; US-06 page 3; US-09 page 5; US-20 page 12; US-21 page 13
- Quote: "And the listing has 10 units available" (US-08 Scenario 1, page 4); "When the recipient requests 12 units" (US-08 Scenario 2, page 5); "When the recipient requests 0 units" (US-08 Scenario 3, page 5)
- Finding: Criteria use Given/When/Then with concrete quantities that decide pass or fail, mirroring the sample story in the course project description (ics613-research/requirements.md, Sample User Stories section). Empty states are covered for search results, the request queue, notifications, and the dashboard.
- Question: none

### F-03: Permission rules captured as scenarios across the story set
- Category: Strength
- Severity: n/a
- Location: Team 4 - 02 - Acceptance Criteria.pdf, permission-rule scenarios across US-04 through US-29
- Quote: "Given a member who does not own the listing" (US-10 Scenario 5, page 6); "Then the system denies access" (US-10 Scenario 5, page 6)
- Finding: Nearly every story carries a deny path for the wrong actor, which is how the instructor asked permissions to be handled (ics613-research/requirements.md, Clarifications section, A1). Suspended-account denials appear across US-02, US-04, US-08, US-13, and US-14.
- Question: none

### F-04: Role list matches the assignment baseline, with a coverage table
- Category: Strength
- Severity: n/a
- Location: Team 4 - 03 - User Roles & Personas.pdf, Roles and Role to story coverage sections, page 1
- Quote: "Each row maps a role to the user stories where that role is the actor." (page 1)
- Finding: Guest, Member, Poster, Recipient, and Admin cover the assignment's baseline roles of member, owner/poster, admin, and guest (ics613-research/requirements.md, Conducting the review section). The role-to-story table makes role coverage checkable at a glance, and the role context paragraph defines remaining quantity and the suspended-account rule once for the whole packet.
- Question: none

### F-05: Open questions and risks tie to specific story IDs
- Category: Strength
- Severity: n/a
- Location: Team 4 - 04 - Open Questions and Assumptions.pdf, page 1; Team 4 - 05 - Known Risks.pdf, page 1
- Quote: "over-claim prevention (US-08, US-10), status transitions (US-10, US-11, US-26, US-16, US-17), and no cancellation after pickup (US-11, US-26)" (Known Risks, page 1)
- Finding: Each open question names its affected stories and use cases, and the business-rule risk names the exact stories whose criteria test each rule. This gives the reader a direct path from a question or risk to the requirement it touches.
- Question: none

### F-06: US-26 calls PICKED_UP terminal and omits COMPLETED from the rejection list
- Category: Ambiguity
- Severity: High
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-26 Scenario 2, page 7; US-17 Scenario 1, page 10; ics613-research/requirements.md, Project: Local Produce Exchange section
- Quote: "Given the request is in terminal status PICKED_UP, DENIED, CANCELLED" (US-26 Scenario 2, page 7); "Then the system changes the request status from PICKED_UP to COMPLETED" (US-17 Scenario 1, page 10)
- Finding: PICKED_UP is not terminal: US-17 moves it to COMPLETED, and the course project description defines the sequence REQUESTED to APPROVED to PICKED_UP to COMPLETED with cancellation allowed only from REQUESTED or APPROVED. The rejection list also omits COMPLETED, so no scenario rejects a cancel attempt on a finished exchange, and a cancel attempt on a REQUESTED request falls between US-26 and US-11 with no covering scenario.
- Question: Which statuses must reject a cancel attempt under US-26, and is PICKED_UP meant to appear in that list even though it is not terminal?

### F-07: US-30 public review history conflicts with US-19 participant-only review access
- Category: Ambiguity
- Severity: High
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-30 Scenario 1, page 4; US-19 Scenario 3, page 12
- Quote: "Then the system shows that member's display name and their review history" (US-30 Scenario 1, page 4); "Given a member who is not a participant in the exchange" (US-19 Scenario 3, page 12); "Then the system denies access" (US-19 Scenario 3, page 12)
- Finding: US-19 denies non-participants access to an exchange's reviews, while US-30 shows any logged-in member another member's review history. If review history includes review text from specific exchanges, the two rules contradict each other. The US-30 note that private exchange details are not shown does not say whether review text counts as private.
- Question: Does review history on the public profile mean full review text, or only a summary such as an average rating?

### F-08: US-26 restore step is garbled and names an undefined quantity
- Category: Ambiguity
- Severity: Medium
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-26 Scenario 1, page 7
- Quote: "Restaurants the quantity back to the listing's quantity" (US-26 Scenario 1, page 7)
- Finding: "Restaurants" appears to be a typo for "Restores", and "the listing's quantity" does not say whether the restore target is the quantity available or the remaining quantity defined in the roles document. OQA-04 asks about the same rule, but the criterion as written cannot be turned into a test.
- Question: Should the step read "Restores the approved quantity back to the listing's remaining quantity"?

### F-09: Submission checks "available" while approval checks "remaining"
- Category: Ambiguity
- Severity: Medium
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-08 Scenario 2, page 5; US-09 Scenario 1, page 5; US-10 Scenario 3, page 6; Team 4 - 03 - User Roles & Personas.pdf, Role context, page 1
- Quote: "And the listing has 10 units available" (US-08 Scenario 2, page 5); "the quantity requested and the quantity remaining" (US-09 Scenario 1, page 5); "And approving this request would push the approved quantity past the remaining quantity" (US-10 Scenario 3, page 6)
- Finding: The submission cap is stated against units available while the approval cap and the queue view use remaining quantity. The packet never says which quantity bounds a new request once approvals have reduced the remaining amount.
- Question: At submission time, is the cap the original quantity available or the current remaining quantity?

### F-10: "Open request" is undefined in the duplicate-request rule
- Category: Ambiguity
- Severity: Medium
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-08 Scenario 4, page 5; US-09 Scenario 1, page 5
- Quote: "Given the recipient already has an open request on this listing" (US-08 Scenario 4, page 5); "Then the system shows the pending requests oldest first" (US-09 Scenario 1, page 5)
- Finding: The duplicate rule says open while the queue scenarios say pending, and neither term is defined. If open means REQUESTED only, a member with an APPROVED request can submit a second request on the same listing.
- Question: Does an open request mean REQUESTED only, or REQUESTED and APPROVED?

### F-11: US-27 outcome sentence is garbled and can be read as its own opposite
- Category: Ambiguity
- Severity: Medium
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-27 Scenario 1, page 14
- Quote: "And the account can then longer log in or take member actions" (US-27 Scenario 1, page 14)
- Finding: A word is missing. "Can no longer log in" blocks the account while "can then log in" restores it, and the step states the user-facing outcome of unsuspending. The neighboring step marks the account active, which suggests the restoring reading, but the sentence needs repair before it can anchor a test.
- Question: Should the step read "And the account can then log in and take member actions"?

### F-12: "Allowed to issue invites" is a rule no story defines or manages
- Category: Ambiguity
- Severity: Medium
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-04 Scenarios 1 and 2, page 2; Team 4 - 04 - Open Questions and Assumptions.pdf, OQA-02, page 1
- Quote: "Given the member is not allowed to issue invites" (US-04 Scenario 2, page 2); "Registration is invite-only, but who issues invite tokens is not decided. Assumed a member may issue invite tokens. Confirm whether any active member can invite, or only an admin." (OQA-02, page 1)
- Finding: US-04's criteria hinge on an allowed-to-invite property that no story grants, revokes, or displays, and OQA-02 records the underlying decision as open while US-04 is in the current milestone. An assumption is standing in for a decision that the criteria already depend on.
- Question: Who grants or revokes a member's ability to issue invites, and which story manages it?

### F-13: US-18's duplicate-review given omits "for this exchange" in one scenario
- Category: Ambiguity
- Severity: Low
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-18 Scenarios 1 and 3, page 11
- Quote: "And the member has not already reviewed the other party" (US-18 Scenario 1, page 11); "Given the member has already reviewed the other party for this exchange" (US-18 Scenario 3, page 11)
- Finding: Scenario 3 scopes the duplicate check to this exchange while Scenario 1 does not. Read strictly, Scenario 1 blocks a member from reviewing the same person across two different completed exchanges.
- Question: Should Scenario 1 read "has not already reviewed the other party for this exchange"?

### F-14: Milestone labels drift across the stories
- Category: Ambiguity
- Severity: Low
- Location: Team 4 - 01 - User Stories.pdf, Milestone lines, pages 1 to 7
- Quote: "Milestone: Current scope (R1)" (US-02, page 1); "Milestone: Full Features (R2)" (US-30, page 2); "Milestone: Follow up scope (R2)" (US-26, page 3); "Milestone: Full features (R2)" (US-13, page 4)
- Finding: Three different labels point at R2: Full features, Full Features, and Follow up scope. A reader cannot tell whether Follow up scope is the same milestone as Full features or a third bucket.
- Question: Are "Follow up scope (R2)" and "Full features (R2)" the same milestone?

### F-15: US-28's already-read given names the member instead of the notification
- Category: Ambiguity
- Severity: Low
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-28 Scenario 2, page 13
- Quote: "Given the member is already marked read" (US-28 Scenario 2, page 13); "Then the system accepts silently" (US-28 Scenario 2, page 13)
- Finding: The given marks the member as read where it means the notification. The outcome wording also leaves open what the member sees, though the following step says nothing changes.
- Question: Should the given read "Given the notification is already marked read"?

### F-16: R1 stories depend on listing creation and notifications planned for R2
- Category: Missing case
- Severity: Medium
- Location: Team 4 - 01 - User Stories.pdf, Milestone lines, pages 2 to 5; Team 4 - 02 - Acceptance Criteria.pdf, US-08 Scenario 1, page 4
- Quote: "Milestone: Current scope (R1)" (US-06, page 2); "Milestone: Full features (R2)" (US-13, page 4); "And the poster is notified of the new request" (US-08 Scenario 1, page 4)
- Finding: Browsing, requesting, approving, and messaging are R1 while creating a listing is R2, so the R1 stories operate on listings nothing in R1 creates. R1 criteria also assert notification outcomes while the notification stories (US-20, US-28) are R2. The packet does not say how R1 will be exercised.
- Question: How will the R1 stories run without listing creation in R1: seeded data, an early slice of US-13, or a milestone change?

### F-17: Suspension effects on in-flight listings and requests are unspecified
- Category: Missing case
- Severity: Medium
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-22 page 13 and US-27 pages 13 to 14; Team 4 - 04 - Open Questions and Assumptions.pdf, page 1
- Quote: none (coverage: no scenario across US-22 and US-27, and no item among OQA-01 through OQA-09, addresses a suspended member's active listings, the queued or approved requests on them, or the member's own approved pickups elsewhere)
- Finding: Suspension blocks login and member actions, but the packet does not say whether a suspended member's listings stay browsable, whether pending requests on them can still be approved, or what happens to the member's approved pickups. Counterparties are left mid-flow with no defined outcome.
- Question: When an account is suspended, are its listings hidden, and are its in-flight requests cancelled or frozen?

### F-18: No scenario covers a pickup window that passes
- Category: Missing case
- Severity: Medium
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-13 Scenario 1, page 8; Team 4 - 04 - Open Questions and Assumptions.pdf, page 1
- Quote: none (coverage: no acceptance criterion across US-13 through US-17, and no item among OQA-01 through OQA-09, covers a pickup window that has ended: listing visibility, queued requests, or an approved request that was never picked up)
- Finding: Every listing requires a pickup window, but the packet never says what the window does after it passes. An approved request that is never picked up holds the remaining quantity with no defined release path.
- Question: What happens to a listing and its requests when the pickup window ends?

### F-19: Photos can be managed but no story displays them
- Category: Missing case
- Severity: Medium
- Location: Team 4 - 01 - User Stories.pdf, US-25, page 7; Team 4 - 02 - Acceptance Criteria.pdf, US-07 Notes, page 4
- Quote: "so that members can see what the item looks like" (US-25, User Stories page 7); "Photos are not part of the current listing detail view." (US-07 Notes, page 4)
- Finding: US-25 stores, replaces, and removes photos and its benefit promises that members can see them, but US-07's note excludes photos from the listing detail view and no other story shows them. The benefit depends on a display feature the backlog does not provide.
- Question: Which story shows listing photos to browsing members, or should the US-25 benefit change?

### F-20: The roles document contains no personas despite its title
- Category: Missing case
- Severity: Low
- Location: Team 4 - 03 - User Roles & Personas.pdf, page 1
- Quote: none (coverage: the document titled User Roles and Personas defines five roles, a role context paragraph, and a coverage table, but no named persona with goals or context appears anywhere in the packet)
- Finding: The assignment item is user roles/personas, and the roles half is complete. Personas, in the sense of named example users with goals, are absent, so the packet leans on abstract roles alone.
- Question: Will personas be added, or is the document title meant to promise roles only?

### F-21: The admin report feature has no defined bounds
- Category: Scope risk
- Severity: Medium
- Location: Team 4 - 01 - User Stories.pdf, US-24, page 7; Team 4 - 02 - Acceptance Criteria.pdf, US-24, page 14
- Quote: "As an admin, I want to generate a basic report about system activity so that I can see how the exchange is being used." (US-24, User Stories page 7); "Then the system computes the report" (US-24 Scenario 1, page 14)
- Finding: Neither the story nor the criteria name a single concrete report, a column, or a filter, so basic reports has no stopping point a test can check. Unbounded reporting features grow during implementation.
- Question: Which two or three concrete reports count as basic for the first release?

### F-22: Photo upload and storage is unaddressed in the risks list
- Category: Scope risk
- Severity: Medium
- Location: Team 4 - 04 - Open Questions and Assumptions.pdf, OQA-07, page 1; Team 4 - 05 - Known Risks.pdf, page 1
- Quote: "Confirm the allowed photo file types and a maximum size or count before building." (OQA-07, page 1)
- Finding: Photo upload is the one feature in the packet that needs file handling, storage and serving decisions, and size enforcement, and the known risks document does not mention media at all (coverage: none of the seven risk rows names photos, uploads, files, or storage). OQA-07 covers file types and size but not where photos live or how they are served.
- Question: Where will photos be stored and served, and should media handling join the risks list with a mitigation?

### F-23: Message delivery expectations are undefined
- Category: Scope risk
- Severity: Low
- Location: Team 4 - 02 - Acceptance Criteria.pdf, US-12 Scenario 1, page 8
- Quote: "Then the system saves the message to the thread" (US-12 Scenario 1, page 8); "And shows it to both parties" (US-12 Scenario 1, page 8)
- Finding: The criteria do not say whether the other party sees a new message on refresh or live. The two readings differ widely in implementation effort, and live delivery would be a real-time feature the rest of the packet avoids.
- Question: Is message delivery on page refresh acceptable, or do members expect live updates?
