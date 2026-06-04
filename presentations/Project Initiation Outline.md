# Project Initiation Presentation: Local Produce Exchange

This is a slide-by-slide outline for the Project Initiation presentation. Each
section below maps to one or more slides. Copy the content into PowerPoint and
adjust as needed.

Content is drawn from the course requirements and the team charter. Items still
marked TODO need a team decision (for example, the meeting day and time, or who
claims each role). Replace them before presenting.

A note on the required content: the course asks the Project Initiation deck to
cover, at a minimum, project objectives, scope, user roles and personas, key use
cases and user stories, acceptance criteria, a project management overview, key
risks and mitigation, project artifacts, a quality assurance plan, milestones,
and next steps. All of those sections are included below.

Reminder from the course: all team members should help prepare the deck, and
each member should present a similar number of slides. The inception
presentation is scheduled for June 9, with about 10 minutes per member plus Q
and A.

---

## Slide 1: Title

- Project: Local Produce Exchange
- Course: ICS 613 - Advanced Software Engineering, Summer 2026
- Team: Team 4
- Team members: Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates
- Date: TODO (presentation is June 9, 2026)

Speaker notes: one sentence on what the product is. The Local Produce Exchange
is an invite-only web app that helps a trusted community share extra produce and
other food before it goes to waste. The simple idea: if one person has too many
tomatoes and another person could use them, the app makes that exchange easy.

---

## Slide 2: Agenda

- Project objectives
- Project scope
- User roles and personas
- Key use cases and user stories
- Acceptance criteria
- Project management overview
- Key risks and mitigation
- Project artifacts
- Quality assurance plan
- Milestones
- Next steps

---

## Slide 3: Project Objectives

Goals of the project:

- Reduce food waste by helping neighbors share surplus produce and food items
  instead of throwing them away.
- Provide a safe, invite-only space where members trust each other.
- Make it simple to post an item, claim a portion of it, coordinate pickup, and
  build trust through reviews.

Value to the client and end users:

- Less wasted food and lower grocery costs for members.
- A reliable claim-and-pickup workflow so people know what they are getting and
  when.
- A reputation system (ratings and reviews) that keeps the community trustworthy.

Measurable success targets (from the team charter):

- Finish 100 percent of must-have user stories and at least 80 percent of the
  full 25 to 30 story backlog by final submission.
- Keep automated tests passing for the core business rules.
- Reach at least 70 percent automated test coverage on backend business logic.
- A teammate on a clean machine can follow the deployment guide and run the app
  in under 30 minutes, with seeded demo data that shows every major state.
- Send 100 percent of changes through reviewed pull requests, with zero direct
  commits to main.

Speaker notes: tie the objective back to a real problem. Surplus food often goes
to waste because there is no easy, trusted way to give it away locally.

---

## Slide 4: Project Scope (In Scope)

What we will build:

- Invite-only registration and login, plus member profiles.
- Create, edit, and deactivate listings with required details: description,
  category (for example fruits, vegetables, baked goods, dairy), quantity
  available, dietary and allergen tags (for example vegan, contains nuts,
  gluten-free), and a pickup window.
- Photo uploads on listings.
- Browse, search, and filter active listings.
- Submit and manage claim requests for a specific quantity.
- The full claim lifecycle: REQUESTED -> APPROVED -> PICKED_UP -> COMPLETED,
  with REQUESTED -> DENIED as the rejection path, and CANCELLED allowed only
  from REQUESTED or APPROVED. Includes protection against over-claiming so
  approved quantities never exceed the remaining quantity.
- A private message thread per exchange for coordinating pickup.
- In-app notifications for key status changes (request submitted, approved,
  denied, picked up, completed).
- Ratings and reviews after an exchange is completed.
- A member dashboard showing active listings, incoming and outgoing requests,
  and exchange history, with actions based on the current status.
- An admin role that can suspend users, deactivate listings (hidden from
  browsing but kept for audit history), and generate basic reports.
- A responsive web app, a deployed app, a README, seeded demo data, and
  automated tests for permissions and the main exchange flow.

Stretch goals (only if time allows):

- Pickup reminders as a listing's pickup window gets close.
- Email or SMS notifications through an external provider.

---

## Slide 5: Project Scope (Out of Scope)

What we will not build:

- Payments, prices, tips, or any paid transactions. The exchange is free.
- Public sign-up. Access stays invite-only.
- Native mobile apps. The web app works on phones, but we are not building
  separate iOS or Android apps.
- Delivery, shipping, maps, or route planning. Pickup is coordinated through the
  message thread and the listing's pickup window.
- Live chat, voice, or video. Messaging is a simple thread for each exchange.
- Recommendation or smart-matching algorithms.
- Multi-language support and formal accessibility certification.
- Production-grade scaling, load handling, and security hardening. This is a
  class project, not a commercial launch.

Speaker notes: being clear about what is out of scope protects the team from
scope creep.

---

## Slide 6: User Roles and Personas

Roles in the system:

- Guest: someone with an invite token who has not registered yet. Can only
  register.
- Member: a registered user. Can post listings, browse and search, submit
  claims, message, and leave reviews.
- Poster: a member acting as the owner of a specific listing. Approves or denies
  claims on their own listing.
- Claimant: a member who requests a quantity from someone else's listing.
- Admin: a privileged user who can suspend users, deactivate listings, and run
  basic reports.

Note: poster and claimant are not separate accounts. They are the role a member
plays for a given exchange. The same person can be a poster on one listing and a
claimant on another.

Personas (sketch two or three to make it concrete):

- TODO: Persona 1, for example "Maria, a home gardener with extra tomatoes."
- TODO: Persona 2, for example "Sam, a member who wants to pick up fresh produce
  near home."

---

## Slide 7: Key Use Cases

High-level use cases:

- Register with a valid invite token.
- Create and manage an item listing.
- Browse, search, and filter active listings.
- Submit a claim for a specified quantity.
- Approve or deny a claim (poster).
- Move an exchange through pickup and completion.
- Cancel a claim (only from REQUESTED or APPROVED).
- Exchange messages within an exchange thread.
- Leave a rating and review after completion.
- Admin: suspend a user, deactivate a listing, generate a report.

Speaker notes: these map directly to the user stories the team is writing for the
requirements packet.

---

## Slide 8: Sample User Stories

User Story 1:

- As a claimant, I want to submit a claim for a specified quantity of a listing
  so that I can request only the amount I need.

User Story 2:

- As a claimant or poster after completion, I want to leave a short rating and
  review of the other party so that the community can build trust.

Note for the team: the full requirements packet needs 25 to 30 user stories that
each represent a distinct new functionality. Do not reach the count by splitting
one feature across roles or scenarios. Edge cases and permission rules belong in
the acceptance criteria of a story, not as separate stories. You can still have
separate stories for the different goals of each user type (for example, "poster
approves a claim" and "claimant submits a claim").

TODO: pick two or three of your strongest stories to show on this slide.

---

## Slide 9: Acceptance Criteria (Example)

For User Story 1 (submit a claim for a specified quantity):

Scenario: Valid claim request

- Given a listing is active
- And the listing has 10 units available
- When the claimant requests 3 units
- Then a claim is created with status REQUESTED
- And the requested quantity is 3

Scenario: Claim quantity exceeds available quantity

- Given a listing is active
- And the listing has 10 units available
- When the claimant requests 12 units
- Then the system rejects the claim
- And no claim is created

Scenario: Claim quantity must be positive

- Given a listing is active
- When the claimant requests 0 units
- Then the system rejects the claim
- And displays a validation error

Speaker notes: point out that edge cases (too many units, zero units) live here
in the acceptance criteria, which is what the instructor clarified.

---

## Slide 10: Project Management Overview (Roles and Responsibilities)

Team members: Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates.

These roles describe who keeps an eye on each area. They are not silos. Everyone
writes code, reviews pull requests, and helps where the project needs it. The
team claims specific roles at the kickoff meeting.

Role framework to assign (TODO: assign a name to each at kickoff):

- Team Lead and Scrum Master: runs meetings, keeps the project board honest,
  tracks milestones and deadlines, watches scope, makes sure deliverables are
  submitted on time, and coordinates presentations. Secondary: documentation and
  backup frontend.
- Backend and Database Lead: owns FastAPI services, SQLAlchemy models, Pydantic
  schemas, the PostgreSQL schema and ERD, migrations, seeded demo data, and the
  core business rules. Secondary: backup DevOps and CI.
- Frontend Lead: owns React and TypeScript, UI components, responsive layout,
  client-side validation, and connecting the UI to the API. Secondary: backup
  QA and UI tests.
- QA and DevOps Lead: owns test strategy, automated tests, manual test cases,
  GitHub Actions CI, and the deployment guide. Secondary: backup backend.

---

## Slide 11: Project Management Overview (Process and Tracking)

Meetings and communication:

- One weekly live meeting, with shorter updates on Discord during the week.
- TODO: confirm the weekly meeting day and time.
- Discord team channel is the main place for project communication, with a
  pinned message holding the GitHub repository URL.
- Meeting notes for every meeting (decisions, action items, owners) kept in a
  shared Google Drive.

Task management and tracking:

- GitHub Issues for every story, task, and bug, each assigned to someone and
  grouped by milestone.
- GitHub Projects board with columns: To Do, In Progress, In Review, Done.
- Milestones to track progress toward key deadlines.
- The board is updated at least weekly.

Development workflow:

- Short-lived branches named like feature/<issue#>-short-description or
  fix/<issue#>-short-description.
- Main is protected. Nobody commits directly to main.
- Every change reaches main through a pull request linked to its issue, with at
  least one teammate review and passing CI checks before merge.
- Clear, present-tense commit messages that include the issue number, for
  example: "Add over-claim validation to claim service (#42)".
- Definition of Done: acceptance criteria met, code reviewed and merged,
  relevant automated tests pass, and docs updated.

---

## Slide 12: Key Risks and Mitigation

- Risk: scope gets too big, especially stretch features (photos, notifications,
  and possible email or SMS plus pickup reminders).
  - Mitigation: build the core exchange flow first, keep stretch features tagged
    and low priority, and defer or cut them through the change-request process
    if we fall behind.
- Risk: the stack has a learning curve (React, TypeScript, FastAPI, PostgreSQL).
  - Mitigation: build a thin end-to-end slice early by R1, pair on hard parts,
    and use lecture material and office hours.
- Risk: workload becomes uneven or someone becomes unavailable.
  - Mitigation: assign a secondary owner for every area, distribute issues
    fairly each milestone, and follow the team's escalation steps.
- Risk: core business-rule bugs sneak in (over-claim prevention, status
  transitions, no cancellation after pickup).
  - Mitigation: write automated tests for these rules early, cover edge and
    negative paths, and require review on any pull request that touches them.
- Risk: setup drifts across machines.
  - Mitigation: everyone sets up their environment in week 1, keep setup steps
    and seeded demo data in the README, and let CI catch breakages early.
- Risk: deadlines get tight.
  - Mitigation: track milestones on the board, review progress weekly, and
    protect must-have work before nice-to-have work.

---

## Slide 13: Project Artifacts

Documents and tools the team will create:

- Team and process: team charter; GitHub repository with README; protected main
  branch; pinned Discord repo link; project board, issues, and milestones;
  meeting notes and decision log; retrospective and defect-triage notes.
- Requirements: high-level use cases; 25 to 30 user stories with acceptance
  criteria; domain model; cross-team requirements review packet and report.
- Design: technical design document with an architecture diagram, components,
  ERD, data model, key API endpoints, technology stack, risks, and tradeoffs.
- Code and quality: source code, automated test suite, manual test cases, QA
  packet, QA summary, and GitHub Actions CI workflows.
- Delivery and release readiness: seeded demo data, validated deployment guide,
  release-readiness document, and presentation decks for inception, R1, R2, and
  final.
- Individual accountability: peer evaluations and individual reflections.

Tools: GitHub (repository, Issues, Projects, Milestones, wiki, Actions for
CI/CD), Discord for communication, and Google Drive for draft docs and meeting
notes. Drafts live in Google Drive; final versions are committed to or linked
from the repository.

---

## Slide 14: Quality Assurance Plan

Approach to quality:

- Automated unit tests for every core business rule: over-claim prevention,
  allowed status transitions, and no cancellation after pickup.
- Tests for every permission rule for members, admins, and guests, and for the
  full exchange flow from listing to completion.
- Coverage goal: at least 70 percent on backend business logic.
- Manual test cases for key flows, covering positive, negative, edge,
  permission, and workflow paths.
- UI automation with Playwright is a stretch goal.

Defect tracking:

- Report and track bugs as GitHub Issues with labels.
- Triage defects during the weekly team meeting.

Code quality and continuous integration:

- Linters and formatters on both stacks: Ruff and Black for Python, ESLint and
  Prettier for TypeScript.
- A GitHub Actions workflow runs the linters on every pull request (Ruff for
  Python, ESLint for TypeScript). These checks must pass before merge, which
  protects main alongside reviews and branch protection.
- Pull request review required before merging to main.

---

## Slide 15: Milestones

Based on the course schedule and the team charter:

- Team and process setup (repo, board, Discord, charter) - charter due June 5,
  2026
- Inception presentation and requirements packet - June 8, 2026 (presented June
  9)
- Cross-team requirements review report - June 15, 2026 (presented June 16)
- Technical design document - July 1, 2026
- R1 demo and presentation - July 6, 2026 (presented July 7; retrospective and
  defect triage July 9)
- Manual test cases / QA packet - July 14, 2026
- R2 demo and presentation - July 27, 2026 (presented July 28)
- Deployment guide and release readiness - August 6, 2026
- Final submission and final presentation - August 11, 2026 (final demos August
  11 to 13; peer evaluation and reflection August 13)

TODO: assign owners for each milestone (currently TBD in the charter).

---

## Slide 16: Next Steps

- Claim team roles at the kickoff meeting and confirm the weekly meeting time.
- Set up the GitHub repository, project board, README stub, and branch
  protection; pin the repo link in Discord.
- Finish and submit the requirements packet (25 to 30 user stories with
  acceptance criteria) by June 8.
- Begin the domain model and prepare for the cross-team requirements review on
  June 15.
- Everyone sets up their development environment in week 1.
- TODO: add any other immediate action items.

---

## Slide 17: Questions

- Thank you. Questions?
- TODO: add the GitHub repository link once it is created.
