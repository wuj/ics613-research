# **Team Charter / Team Contract**

| Team Name | Team 4 |  |  |
| :---- | :---- | :---- | :---- |
| **Project Name** | Local Produce Exchange |  |  |
| **Course** | ICS613 | **Semester** | Summer 2026 |

# **1\. Team Purpose**

Team 4 is building an online local produce exchange web application. This produce exchange is intended to serve an invite-only community by helping them share extra produce and other food before it goes to waste. The basic idea is this: if one person has too many tomatoes and another person could use them, the app should make that exchange easy.

We are also using this project as an opportunity to practice our software engineering skills as a team via the software development lifecycle. That means we will gather requirements, design the system, build features, test our work, and deliver working software through regular milestones.

# **2\. Project Scope and Objectives**

## **Project Vision**

Local Produce Exchange is an invite-only web application where people in a trusted community can post extra food items and claim items from one another. The goal is to turn food that might have been wasted into food that gets used.

## **Goals**

*Add 3 to 5 measurable or concrete project goals.*

* **Story completion:** finish 100 percent of must-have user stories and at least 80 percent of the full 25 to 30 story backlog by final submission, delivered across the R1 and R2 milestones.
* **Business-rule correctness:** keep automated tests passing for each core rule: no approved claim can exceed a listing's remaining quantity, claim statuses must move only through allowed transitions, and a claim cannot be cancelled after pickup.
* **Test coverage:** reach at least 70 percent automated test coverage on backend business logic, plus tests for every permission rule for members, admins, and guests, and for the full exchange flow from listing to completion.
* **Reproducible deployment:** make sure teammates on a clean machine can follow the deployment guide and get the app running in under 30 minutes, with seeded demo data that shows every major state.
* **Process discipline:** send 100 percent of changes through reviewed pull requests, allow zero direct commits to main, and update the project board at least weekly.
* **Maintain clear documentation:** produce and maintain thorough project documentation, including requirements, system designs, user stories, and meeting notes, to support future development and team communication.

## **In Scope**

*List the features, responsibilities, or deliverables the team is committing to.*

* Invite-only registration and login, plus member profiles
* Create, edit, and deactivate listings with required details: description, category, quantity available, dietary and allergen tags, and a pickup window
* Browse, search, and filter active listings
* Submit and manage claim requests for a specific quantity
* A request queue so claim requests are handled in order, without conflicts
* Full claim lifecycle: REQUESTED, APPROVED, PICKED_UP, COMPLETED, DENIED, and CANCELLED, including protection against over-claiming
* A private message thread for each exchange (poster to recipient) so members can coordinate pickup
* Ratings and reviews after an exchange is completed
* A member dashboard showing active listings, incoming and outgoing requests, and exchange history, with actions based on the current status
* An admin role that can suspend users, deactivate listings without deleting audit history, and generate basic reports
* A database schema that acts as the single source of truth for listings, message threads, users, reviews, and related data
* A responsive UI, deployed app, README, seeded demo data, and automated tests for permissions and the main exchange flow
* Photo uploads on listings
* In-app notifications for key status changes: request submitted, approved, denied, picked up, and completed
* Stretch: pickup reminders as a listing's pickup window gets close
* Stretch: email or SMS notifications through an external provider

## **Out of Scope**

*List major items the team is intentionally not taking on.*

* Payments, prices, tips, or any paid transactions. This exchange is free.
* Public sign-up. Access stays invite-only.
* "Looking for" posts. Posters only offer up what they have, so the app does not support requests for specific items.
* Native mobile apps. The web app should work well on phones, but we are not building separate iOS or Android apps.
* Delivery, shipping, maps, or route planning. Pickup is coordinated through the message thread and the listing's pickup window.
* Live chat, voice, or video. Messaging is a simple thread for each exchange.
* Recommendation or smart-matching algorithms.
* Multi-language support and formal accessibility certification.
* Production-grade scaling, load handling, and security hardening. This is a class project, not a commercial launch.

# **3\. Team Members and Roles**

*List each member, their role, and the responsibilities they will primarily own.*

These roles describe who keeps an eye on each area. They are not silos. Everyone writes code, reviews pull requests, and helps where the project needs it. Secondary responsibilities give us backup so work does not stop when one person is busy. The assignments below are a proposed starting point, based on what each person has already volunteered for; we will confirm them at our first meeting.

| Team Member | Role | Primary Responsibilities | Secondary Responsibilities |
| ----- | ----- | ----- | ----- |
| Kennan Kaneshiro | Backend Lead | Backend code in Python, SQL, and TypeScript that connects the frontend to the backend server and database; FastAPI services, SQLAlchemy models, Pydantic schemas, and the core business rules | Creating and organizing project documentation for maintainable development |
| Shea Stevens | Database Lead | Database administration: the PostgreSQL schema and ERD, migrations, and seeded demo data | Front end system design |
| Matt Ong | Frontend Lead | React and TypeScript UI components, responsive layout, client-side validation, and connecting the UI to the API | Backup QA and UI tests |
| Kim Cates | Team Lead and Scrum Master | Runs meetings, keeps the project board current, tracks milestones and deadlines, watches scope, makes sure deliverables ship on time, and coordinates presentations | Documentation and backup front end |
| Jeff Wu | QA and DevOps Lead | The Docker environment for PostgreSQL, Make and a makefile for easy local builds, GitHub Actions for linting (Ruff, ESLint with typescript-eslint) on each PR, test strategy, and the deployment guide | TypeScript front end functionality and Python as needed |

## **Role Framework**

* **Team Lead and Scrum Master:** runs meetings, keeps the project board honest, tracks milestones and deadlines, watches scope, makes sure deliverables are submitted on time, and coordinates presentations. Secondary: documentation and backup front end.
* **Backend Lead:** owns FastAPI services, SQLAlchemy models, Pydantic schemas, the API surface, and the core business rules. Secondary: backup database and documentation.
* **Database Lead:** owns the PostgreSQL schema and ERD, migrations, seeded demo data, and database administration. Secondary: front end system design.
* **Frontend Lead:** owns React and TypeScript, UI components, responsive layout, client-side validation, and connecting the UI to the API. Secondary: backup QA and UI tests.
* **QA and DevOps Lead:** owns test strategy, automated tests, manual test cases, the Docker environment, GitHub Actions CI and linting, and the deployment guide. Secondary: backup back end.

# **4\. Team Values**

*Choose 4 to 6 values that describe how the team wants to work together.*

* **Talk early and honestly:** if something is confusing, blocked, broken, or bigger than expected, we say so early and help each other figure it out.
* **Accountability:** given everyone's busy schedules, keeping deliverables on schedule helps the rest of the team stay on track, so we follow through on our commitments and say something early when a deadline is at risk.
* **Flexibility:** things do come up, so when problems happen we work together as a team to fix them as they occur.
* **Make room for everyone:** every teammate should have a real voice in decisions, and we assume good intent.
* **Build work we can demo:** we do not merge code we would be nervous to show. We test, review, and clean up before calling work done.
* **Keep learning:** we are here to get better at React, Python, PostgreSQL, testing, and teamwork. Mistakes are useful when we learn from them.
* **Review the work, not the person:** feedback should be specific, respectful, and paired with a useful suggestion when possible.

# **5\. Communication Plan**

*Document the tools, response expectations, and meeting cadence the team will use.*

We will have one weekly live meeting and use Discord for shorter updates during the week.

| Purpose | Tool / Channel | Notes |
| ----- | ----- | ----- |
| Quick team communication | Discord team project channel | Main place for project communication. A pinned message holds the GitHub repo URL and Google Drive shared folder links. |
| Task tracking | GitHub Issues and Projects board | Every story, task, and bug is an issue, assigned to someone, grouped by milestone, and tracked on the board. |
| Code repository | [GitHub](https://github.com/kennank27/ICS613_Summer2026_FinalProject_Team4) | One shared repo. We use branches, pull requests, and reviews before merging to main. |
| Meeting notes | [Google Docs shared folder](https://drive.google.com/drive/folders/1s7VIsUajSrPqejtY_euqD7INBDaIAaNj?usp=drive_link) | Notes for every meeting, including decisions, action items, and owners. |
| Documentation and notes | Google Docs / Google Drive | Charter, requirements, and design docs start here. Final versions are committed to or linked from the repo for grading. |

# **6\. Working Agreements**

*Describe expectations for participation, reliability, responsiveness, and workload sharing.*

* **Show up and stay in the loop:** attend the weekly meeting and post short Discord updates on the days we agree to. If you cannot make a meeting, say so ahead of time and read the notes afterward.
* **Reply within a reasonable time:** answer Discord messages within 24 hours on weekdays. If a teammate is blocked by your answer, reply the same day when possible. Acknowledge issues assigned to you within 24 hours.
* **Keep commitments realistic:** we each take a fair share of work for each milestone, keep our GitHub issues current, and speak up early if there is a problem or something might slip.
* **Share the useful and less glamorous work:** everyone should get meaningful coding, testing, and documentation tasks. We will rotate chores like note-taking and cleanup.
* **Bring real progress to meetings:** push work in progress before the weekly meeting so discussion and review are based on actual code, not memory.

# **7\. Decision-Making Process**

*Explain how the team will make technical, scope, and scheduling decisions.*

* **Start with consensus:** most of our decisions should come from talking it through in a meeting or on Discord until the team agrees.
* **Technical decisions:** architecture, libraries, API design, and database design are proposed in a GitHub issue or meeting. If the team cannot agree, the lead for that area makes the call and records the reason.
* **Scope decisions:** adding or cutting a feature affects everyone, so the team has to agree. The Team Lead records major scope changes as change requests, updates the board, and revises this charter if needed.
* **Scheduling decisions:** ownership and target dates are set in the weekly meeting and tracked on the board. The Team Lead resolves conflicts when needed.
* **Tie-breaker:** if a vote splits two to two, the Team Lead breaks the tie and records the decision and the reason. For a purely technical call, the Team Lead may defer to the lead for that area. Important decisions go into the meeting notes so we do not have to rely on memory.

# **8\. Development Workflow**

*Describe how work will be tracked, how branches and pull requests will be used, and what "done" means.*

* **Work tracking:** every task, user story, and bug gets a GitHub issue with an owner and a spot on the Projects board: To Do, In Progress, In Review, or Done.
* **Branching:** work happens on short-lived branches tied to a GitHub issue, named like `feature/<issue#>-short-description` or `fix/<issue#>-short-description`. Nobody commits directly to main. Main is protected.
* **Pull requests and review:** every change reaches main through a pull request linked to its issue. At least one other teammate reviews and approves it before merge. CI checks must pass once GitHub Actions is set up.
* **Commit messages:** use clear, present-tense messages that say what changed and why, and include the issue number when possible. Example: `Add over-claim validation to claim service (#42)`.
* **Definition of Done:** a story is done when its acceptance criteria are met, the code is reviewed and merged to main, the relevant automated tests pass, and any needed docs are updated.

# **9\. Quality Standards**

*Define your expectations for code quality, testing, documentation, and deployment readiness.*

* **Code quality:** use a linter and formatter on both stacks, such as Ruff and Black for Python and ESLint and Prettier for TypeScript. Keep functions small, names clear, and commented-out dead code out of main.
* **Testing:** write automated unit tests for every core business rule: over-claim prevention, allowed status transitions, and no cancellation after pickup. Also test every permission rule. Hold the team's coverage goal of at least 70 percent on backend business logic. Write manual test cases for key flows. UI automation with Playwright is a stretch goal.
* **Documentation:** keep the README current with setup, run, test, and deploy steps. Document the API endpoints and data model. Capture decisions in meeting notes.
* **Deployment readiness:** keep seeded demo data and a tested deployment guide so the app can be brought up cleanly from a fresh checkout.
* **Continuous integration:** a GitHub Actions workflow runs our linters on every pull request, Ruff for the Python code and ESLint for the TypeScript code. These checks must pass before a pull request can merge, which protects main alongside reviews and branch protection.

# **10\. Project Artifacts**

*List the project artifacts the team commits to maintaining during the course.*

* **Team and process:** team charter; GitHub repository with README; protected main branch; pinned Discord repo link; project board, issues, and milestones; meeting notes and decision log; retrospective and defect-triage notes.
* **Requirements:** high-level use cases; 25 to 30 user stories with acceptance criteria; domain model; cross-team requirements review packet and report.
* **Design:** technical design document with an architecture diagram, components, ERD, data model, key API endpoints, technology stack, risks, and tradeoffs.
* **Code and quality:** source code, automated test suite, manual test cases, QA packet, QA summary, and GitHub Actions CI workflows.
* **Documentation:** project documentation that details how the system is designed, its features, database schemas, and so on, for easy management and future development.
* **Delivery and release readiness:** seeded demo data, validated deployment guide, release-readiness document with implemented features and known limitations, and presentation decks for inception, R1, R2, and final.
* **Individual accountability:** peer evaluations and individual reflections.

Drafts live in Google Drive. Final versions are committed to or linked from the repo.

# **11\. Conflict Resolution and Accountability**

*Explain what the team will do if someone is unresponsive, misses work, or if a conflict arises.*

* **Talk to each other first:** if there is a disagreement or someone is falling behind, the people involved talk directly and respectfully as early as possible.
* **Bring in the team when needed:** if a direct conversation does not fix the issue, raise it in the weekly meeting so the team can agree on next steps.
* **Handle silence or missed work quickly:** if a member goes quiet or misses a commitment, the team reaches out on Discord. If there is no response within two days, the team reassigns the at-risk work to keep the project moving and records it in the meeting log.
* **Escalate repeated or serious issues:** if a problem keeps coming back or cannot be resolved inside the team, we raise it with the instructor. Contributions are visible through GitHub activity and reflected honestly in peer evaluations.
* **Stay problem-focused:** conflict is something to solve together, not a reason to blame someone.

# **12\. Risks and Mitigation**

*Identify likely risks and write how the team will reduce or manage them.*

* **Scope gets too big, especially stretch features.** Photos and notifications are already ambitious, and email or SMS plus pickup reminders add extra moving parts. Mitigation: build the core exchange flow first, keep stretch features tagged and low priority, and defer or cut them through the change-request process if we fall behind.
* **The stack has a learning curve.** Some members may be new to React, TypeScript, FastAPI, or PostgreSQL. Mitigation: build a thin end-to-end slice early by R1, pair on hard parts, and use lecture material and office hours.
* **Workload becomes uneven or someone becomes unavailable.** Mitigation: use secondary owners for every area, distribute issues fairly each milestone, and follow the escalation steps in Section 11.
* **Core business-rule bugs sneak in.** The highest-risk rules are over-claim prevention, status transitions, and no cancellation after pickup. Mitigation: write automated tests for these rules early, cover edge and negative paths, and require review on any pull request that touches them.
* **Setup drifts across machines.** Mitigation: everyone sets up their environment in week 1, we keep setup steps and seeded demo data in the README, and CI catches breakages early.
* **Deadlines get tight.** Mitigation: track milestones on the board, review progress in the weekly meeting, and protect must-have work before nice-to-have work.

# **13\. Milestones and Timeline**

*Capture major checkpoints and target dates.*

| Milestone | Target Date | Owner(s) | Notes |
| ----- | ----- | ----- | ----- |
| Team Formation (repo, board, Discord) | June 1, 2026 | TBD | Teams, repo, and Discord due June 1 |
| Team Charter | June 5, 2026 | TBD | Charter due June 5 |
| Inception Presentation | June 8, 2026 | TBD | Presented June 9; each member about 10 minutes plus Q and A |
| Core Requirements Artifacts | June 8, 2026 | TBD | Core requirements and requirements packet |
| Cross-Team Requirements Review Packet | June 15, 2026 | TBD | Presented June 16 |
| Technical Design Document | July 1, 2026 | TBD | Architecture and components, ERD, API endpoints, tech stack, risks |
| R1 Demo + Presentation | July 6, 2026 | TBD | Presented July 7; retrospective and defect triage July 9 |
| Manual Test Cases / QA Packet | July 14, 2026 | TBD | Positive, negative, edge, permission, and workflow tests |
| R2 Demo + Presentation | July 27, 2026 | TBD | Presented July 28 |
| Deployment Guide + Release Readiness | August 6, 2026 | TBD | Deployment steps, seeded data, known limitations |
| Final Submission + Final Presentation | August 11, 2026 | TBD | Final demos August 11 to 13; peer evaluation and reflection August 13 |

# **14\. Team Commitment Statement**

We agree to show up, communicate clearly, do our share, and help each other finish this project well. We understand that this course grades both the software we deliver and the engineering process we use to build it. Because of that, we commit to steady collaboration, clear documentation, responsible project management, and technical work we are willing to stand behind.

# **15\. Signatures**

*Each member should acknowledge the charter with a name and date.*

| Name | Signature / Acknowledgment | Date |
| ----- | ----- | ----- |
| Kennan Kaneshiro | [Acknowledge] | [Date] |
| Shea Stevens | [Acknowledge] | [Date] |
| Matt Ong | [Acknowledge] | [Date] |
| Kim Cates | [Acknowledge] | [Date] |
| Jeff Wu | [Acknowledge] | [Date] |
