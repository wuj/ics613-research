# Plan: High-level Use Cases for Local Produce Exchange

## Context

The "High-level use cases" artifact is a Core Requirement for the inception
deliverable due June 8 (requirements.md:148; meeting-notes 2026-06-02). It is
also required content for the Project Initiation deck (Slide 7 already sketches a
key-use-case list) and for the Cross-Team Requirements Review Packet. Right now
the team has a charter, the requirements doc, and a use-case *template*, but no
actual use-case document. This plan produces that document.

The goal: synthesize the charter (In Scope product-behavior items, goals, roles),
the project requirements (claim lifecycle, business rules, roles), and the
Project Initiation Outline into one complete set of high-level use cases that
(a) covers the product behavior the team committed to in the charter, (b) meets
every functional project requirement, and (c) strictly follows
`artifacts/use-case-template.md`. Charter commitments that are not actor goals,
such as responsive UI, deployment, README, seeded demo data, automated tests,
test coverage, and pull-request discipline, must be listed separately as
"covered outside this use-case artifact" and traced to the appropriate later
artifact.

Use the PDFs in `ics613-research/lectures` while drafting the content. In
particular, apply the course guidance on writing requirements that are correct,
unambiguous, complete, consistent, verifiable, and traceable; keeping
requirements focused on what the system must do; avoiding vague terms and
unrequested features; and reviewing AI-assisted requirements work before the team
accepts it. The finished use-case document should reflect those ideas through
its structure, wording, traceability, and verification checks, without adding a
separate explanation that announces this source basis.

Three working decisions were made for the draft. Each one must be labeled as an
assumption in `use-cases.md` until the team approves it:
- Invitations: the source files require invite-only registration, but they do not
  say who creates or sends invite tokens. Include `UC-04 Invite a new member`
  only as an assumption, and add this open question: "Who can issue invite
  tokens: admin, member, or seeded setup only?" If the team does not decide,
  keep `UC-01 Register with an invite token` as core and mark `UC-04` as
  optional or pending.
- Lifecycle ownership: the source files require the states REQUESTED, APPROVED,
  PICKED_UP, COMPLETED, DENIED, and CANCELLED, but they do not say which actor
  owns the PICKED_UP and COMPLETED transitions. Use the working assumption that
  the Claimant confirms pickup (PICKED_UP) and the Poster marks completion
  (COMPLETED), but include it in the assumptions/open questions table.
- Terminal statuses: explicitly state that DENIED and CANCELLED are terminal
  claim statuses.
- Stretch features (pickup reminders, email/SMS): include them as stretch UCs
  only, clearly labeled as stretch goals.

## Output

A single new file: `ics613-research/artifacts/use-cases.md`

Structure (mirrors the template's parts):
1. Short intro: what this artifact is, the roles it uses, and a pointer back to
   the template and source documents. Include one line noting that the UC
   numbering here is independent of the template's illustrative worked example
   (which uses UC-04 for "Claim produce from a listing",
   use-case-template.md:147,204), so reviewers do not expect a match.
2. Actors/roles list (Guest, Member, Poster, Claimant, Admin) with one-line
   definitions, taken from requirements.md:33-35,341 and the Initiation Outline
   Slide 6 (poster/claimant are roles a member plays, not separate accounts).
3. The full set of use cases, each using the exact template block from
   use-case-template.md:60-79 (ID + verb-phrase name, primary actor, supporting
   actors, goal, preconditions, trigger, main success flow, alternate/exception
   flows, postconditions, related user stories).
4. A use-case diagram section included in the file. For strict template
   compliance, include a formal UML-style PlantUML source block with actors
   outside the system boundary and use cases inside the boundary, matching
   use-case-template.md:124-138 and 188-193. If a rendered image is not added,
   state that the PlantUML block is the formal diagram source. A Mermaid
   `graph LR` approximation may also be included for readability, but it must be
   labeled as a readable approximation, not the formal UML use-case diagram.
5. Part 6 master summary table (ID, name, primary actor, one-line goal).
6. A role-coverage check, a charter-scope-to-UC traceability table, and a
   requirements-to-UC traceability table so reviewers can confirm nothing was
   dropped.
7. An assumptions and open questions table. Include at least the invite-token
   issuer question, lifecycle ownership for PICKED_UP and COMPLETED, and the
   exact report types for admin reports.
8. A non-use-case commitment table for charter items that belong to other
   artifacts, such as responsive UI, deployment, README, seeded demo data,
   automated tests, test coverage, and pull-request discipline.

## Master use-case list (the synthesis to review)

23 core use cases + 2 stretch = 25. Every charter In-Scope product-behavior item
and every functional requirement maps to at least one. Non-use-case commitments
map to other project artifacts. Detail like exact validation messages and every
branch stays out (it belongs in the user stories, per use-case-template.md:29-31).

Authentication and account:
- UC-01 Register with an invite token; primary actor: Guest
- UC-02 Log in; primary actor: Member
- UC-03 Log out; primary actor: Member
- UC-04 Invite a new member; primary actor: Member (assumption; exact
  invite-token issuer is
  an open question)
- UC-05 Manage member profile; primary actor: Member

Listings (poster):
- UC-06 Create a listing (description, category, quantity, dietary/allergen tags,
  pickup window; optional photos as an alternate flow); primary actor: Poster
- UC-07 Edit a listing (including adding, replacing, or removing optional
  photos); primary actor: Poster
- UC-08 Deactivate own listing; primary actor: Poster

Discovery (member):
- UC-09 Browse, search, and filter active listings; primary actor: Member
- UC-10 View listing details; primary actor: Member

Claims, claimant side:
- UC-11 Claim produce from a listing (rejects a single request that exceeds the
  total quantity available, per Sample US#1 Scenario 2 at requirements.md:56-61;
  rejects zero or negative quantity, per requirements.md:63-67; notifies the
  poster when a request is submitted); primary actor: Claimant
- UC-12 Confirm pickup (APPROVED -> PICKED_UP; assumed claimant-owned
  transition; notifies the poster); primary actor: Claimant
- UC-13 Cancel a claim (only from REQUESTED or APPROVED; notifies the poster);
  primary actor: Claimant

Claims, poster side:
- UC-14 Approve or deny a claim request (REQUESTED -> APPROVED or DENIED;
  enforces that cumulative approved quantity never exceeds the remaining
  quantity when multiple claims compete, per requirements.md:33; notifies the
  claimant); primary actor: Poster
- UC-15 Complete an exchange (PICKED_UP -> COMPLETED; assumed poster-owned
  transition; notifies the claimant); primary actor: Poster

Coordination and trust:
- UC-16 Coordinate pickup via the exchange message thread; primary actor: Member
  (both parties)
- UC-17 View status notifications (member-initiated; opens their notifications.
  The generation of each notification stays as a step or postcondition inside
  UC-11, 12, 13, 14, and 15, not here); primary actor: Member
- UC-18 Leave a rating and review after completion; primary actor: Member (as
  reviewer; covers both poster-reviews-claimant and claimant-reviews-poster;
  successful flow or postconditions must also make the saved review visible to
  the reviewed party, per requirements.md:82,91)
- UC-19 View reviews for a completed exchange; primary actor: Member (explicitly
  covers the reviewed party's ability to view the saved review, per
  requirements.md:82,91)
- UC-20 View my dashboard and activity overview; primary actor: Member (a single
  viewing goal: see my active listings, incoming requests, outgoing requests, and
  exchange history, with the status-based actions surfaced as entry points to
  their own use cases UC-08, 12, 13, 14, 15 rather than performed here; matches
  the profile-view vs action-dashboard split in requirements.md:35)

Admin:
- UC-21 Suspend a user; primary actor: Admin
- UC-22 Deactivate a listing as admin (hidden from browsing, audit history kept);
  primary actor: Admin
- UC-23 Generate basic reports; primary actor: Admin (name one or two concrete
  report types as an assumption so the UC is verifiable, for example
  active-listing count and completed-exchange count; flag the exact report set as
  an open question for the cross-team review packet, per
  use-case-template.md:111,185 and requirements.md:332)

Stretch (clearly labeled):
- UC-24 View pickup reminders (stretch; reminder generation stays a system step,
  the member-initiated goal is viewing them); primary actor: Member
- UC-25 Manage email or SMS notification delivery (stretch; member opts in and
  views externally delivered notifications); primary actor: Member

## Charter / requirement coverage map (to include in the doc)

- Invite-only registration, login, profiles -> UC-01, 02, 03, 04, 05
- Create/edit/deactivate listings with required details -> UC-06, 07, 08
- Photo uploads -> UC-06 (optional during create), UC-07 (add, replace, remove)
- Browse, search, filter -> UC-09, 10
- Submit and manage claims -> UC-11, 12, 13, 14, 15
- Full claim lifecycle + over-claim protection + no cancel after pickup
  -> UC-11 (request cannot exceed total available), 12, 13 (cancel limited to
  REQUESTED/APPROVED), 14 (cumulative approved cannot exceed remaining), 15
- Private message thread per exchange -> UC-16
- In-app notifications for request submitted, approved, denied, cancelled,
  picked up, and completed -> UC-11, 12, 13, 14, 15, 17. Cancellation is included
  because requirements.md:33 says notifications are generated for status changes,
  while team-charter.md:41 names the minimum key status changes.
- Ratings and reviews after completion -> UC-18 (leave review and make the saved
  review visible to the reviewed party) and UC-19 (view reviews for a completed
  exchange).
- Member dashboard (active listings, incoming and outgoing requests, history;
  status-based actions reached from here) -> UC-20 (view) plus the action UCs
  08, 12, 13, 14, 15
- Admin: suspend users, deactivate listings, reports -> UC-21, 22, 23
- Stretch: pickup reminders -> UC-24
- Stretch: email/SMS notifications -> UC-25

Non-use-case charter commitments to trace outside this artifact:
- Responsive UI -> UI implementation standards and manual/automated UI checks.
- Deployed app and deployment under 30 minutes -> deployment guide and release
  readiness document.
- README -> README artifact.
- Seeded demo data that shows every major state -> seed data and demo plan.
- Automated tests, permission tests, exchange-flow tests, and 70 percent backend
  business-logic coverage -> QA plan, automated test suite, and QA summary.
- Pull requests, no direct commits to main, weekly board updates -> project
  process artifacts, GitHub repository settings, meeting notes, and project board.

Role coverage (template checklist, use-case-template.md:190-191): Guest=UC-01;
Member=UC-02/03/04/05/09/10/16/17/18/19/20/24/25;
Poster=UC-06/07/08/14/15; Claimant=UC-11/12/13; Admin=UC-21/22/23. All five
roles are the primary actor of at least one use case.

## Writing rules to follow

- Adhere to the template block exactly (use-case-template.md:60-79) and run the
  Part 5 checklist (lines 172-193) against every UC before finishing.
- Keep flows high-level; push validation detail and edge branches into the
  "alternate and exception flows" bullets only as names, not expansions.
- Name every UC with an active, actor-initiated goal and give it a trigger the
  actor starts. Do not frame a UC as passively receiving something the system
  pushes; the system generating a notification or reminder is a step or
  postcondition inside the UC that caused it, not a UC of its own.
- Each UC states one complete goal (use-case-template.md:43). Where a screen
  shows several things and offers several actions (the dashboard), make the UC
  the viewing goal and link the actions out to their own UCs rather than bundling
  them.
- Include a formal UML-style PlantUML use-case diagram source block with the
  system boundary, actors, and use-case ovals. If a Mermaid `graph LR` flowchart
  is also included, split it into readable groups and label it as a readable
  approximation only.
- Add important alternate and exception flows for permissions, ownership, and
  suspended-user access where they apply. Examples: a member cannot edit another
  member's listing, a non-participant cannot review an exchange, a suspended
  member cannot create listings or claims, and an admin-only action is denied to
  non-admin users. Keep these branches brief; detailed scenarios belong in user
  stories and acceptance criteria.
- Each UC lists Related user stories as "TBD" (the 25-30 user stories are the
  next deliverable and are not written yet); note that the UC IDs are the
  traceability anchor those stories will point back to.
- ASCII-only, plain-spoken prose, straight quotes, no em-dashes or non-ASCII, per
  CLAUDE.md output rules.
- Cite the template, charter lines, and requirements lines when useful for
  traceability.
- Use the lecture PDFs as drafting guidance, but do not add a separate section
  explaining that the document follows lecture material.
- Do not include admin invite revocation or user reinstatement as core behavior
  unless a new team decision source is added. If kept, mark it as an assumption
  or future enhancement.

## Verification

- Re-read the finished `use-cases.md` against the Part 5 checklist
  (use-case-template.md:172-193): every UC has an ID + verb-phrase name, a single
  project-defined primary actor, one-sentence goal, preconditions, trigger,
  numbered main success flow, named alternate/exception flows, postconditions,
  and a related-stories line; no two UCs contradict; the formal UML-style
  PlantUML diagram source is present.
- Confirm the coverage map accounts for every charter In-Scope product-behavior
  bullet (team-charter.md:30-38, 40-43) and every lifecycle state in
  requirements.md:33.
- Confirm non-use-case charter commitments from team-charter.md:22-26 and
  team-charter.md:39 are traced to non-use-case artifacts rather than forced into
  actor-goal use cases.
- Confirm all five roles appear as a primary actor (role-coverage check above).
- Confirm each UC states one complete goal: UC-20 is a viewing goal that links
  out to the action UCs (08, 12, 13, 14, 15) instead of bundling them, and UC-17,
  24, and 25 are member-initiated viewing or opt-in goals, not passive "receive"
  use cases.
- Confirm review viewing is traceable to UC-19, not only review writing in
  UC-18, per requirements.md:82,91.
- Confirm UC-23 names at least one concrete report type as an assumption and
  flags the exact report set as an open question.
- Confirm UC-11 (request cannot exceed total available) and UC-14 (cumulative
  approved cannot exceed remaining) state the two over-claim checks distinctly.
- Confirm the formal UML-style PlantUML use-case diagram source is present,
  includes a system boundary, actors, use-case ovals, and the same actors and UC
  IDs as the master list. If a Mermaid flowchart is also included, confirm it is
  labeled as a readable approximation.
- Confirm the intro notes that UC numbering is independent of the template's
  illustrative UC-04 example.
- Confirm the final artifact follows the course guidance for clear, testable,
  traceable requirements without adding a separate explanation about following
  course material.
- Confirm the finished document includes an assumptions/open questions table with
  the invite-token issuer, lifecycle ownership for PICKED_UP and COMPLETED, and
  admin report types.
- Confirm every relevant UC names brief permission, ownership, and suspended-user
  exception flows.
- Review the completed use case document with the team, record approved
  assumptions and open questions, and update the traceability tables before
  submission. This follows the Week 02 validation guidance that stakeholders
  inspect requirement artifacts for errors and gaps, and the Week 01c AI guidance
  to critically evaluate AI-assisted output instead of accepting it blindly.
- Spot-check ASCII-only and plain-language compliance.
