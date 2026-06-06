# **Team Charter / Team Contract**

| Team Name | Team 4 |  |  |
| :---- | :---- | :---- | :---- |
| **Project Name** | Local Produce Exchange |  |  |
| **Course** | ICS613 | **Semester** | Summer 2026 |

# **1\. Team Purpose**

Team 4 is building an online local produce exchange web application called "Surplus". This produce exchange is intended to serve an invite-only community by helping them share extra produce and other food before it goes to waste. The basic idea is this: if one person has too many tomatoes and another person could use them, the app should make that exchange easy.

We are also using this project as an opportunity to practice our software engineering skills as a team via the software development lifecycle. That means we will gather requirements, design the system, build features, test our work, and deliver working software through regular milestones.

# **2\. Project Scope and Objectives**

## **Project Vision**

Local Produce Exchange is an invite-only web application where people in a trusted community can post extra food items and claim items from one another. The goal is to turn food that might have been wasted into food that gets used.

## **Goals**

*Add 3 to 5 measurable or concrete project goals.*

* **Reproducible deployment:** Make sure teammates on a clean machine can follow the deployment guide and get the app running in under 30 minutes, with seeded demo data.
* **Maintain clear documentation:** Produce and maintain comprehensive project documentation including requirements, system designs, user stories, meeting notes to support future development and team communication.
* **Deliver working end to end Demonstration:** Effectively demonstrate a complete produce exchange portal by walking through the full use case of posters and claimers through the following states: Requested, Approved, Picked Up, Completed.
* **Maintain testing threshold:** Ensure with edge cases and error path testing
* **Story completion:** Finish 100 percent of must-have user stories and at least 80 percent of the full 25 to 30 story backlog by final submission, delivered across the R1 and R2 milestones.
* **Business-rule correctness:** Keep automated tests passing for each core rule: no approved claim can exceed a listing's remaining quantity, claim statuses must move only through allowed transitions, and a claim cannot be cancelled after pickup.
* **Test coverage:** Reach at least 70 percent automated test coverage on backend business logic, plus tests for every permission rule for members, admins, and guests, and for the full exchange flow from listing to completion.
* **Process discipline:** Send 100 percent of changes through reviewed pull requests, allow zero direct commits to main, and update the project board at least weekly.

## **In Scope**

*List the features, responsibilities, or deliverables the team is committing to.*

* Invite-only registration and login, plus member profiles
* Browse, search, and filter active listings
* Request Queue to ensure requests are handled in order without conflicts
* Full claim lifecycle: REQUESTED, APPROVED, PICKED_UP, COMPLETED, DENIED, and CANCELLED
* Database schema, supporting single source of truth for listings, threads, users, reviews, etc
* Private message thread for each exchange (poster -> recipient)
* Ratings and reviews after an exchange is completed
* Photo uploads on listings
* An admin role
* Dietary Restrictions Tags for posts
* Key status change notifications
* Create, edit, and deactivate listings with required details: description, category, quantity available, dietary and allergen tags, and a pickup window
* Submit and manage claim requests for a specific quantity
* A member dashboard showing active listings, incoming and outgoing requests, and exchange history, with actions based on the current status
* A responsive UI, deployed app, README, seeded demo data, and automated tests for permissions and the main exchange flow
* Stretch: pickup reminders as a listing's pickup window gets close
* Stretch: email or SMS notifications through an external provider

## **Out of Scope**

*List major items the team is intentionally not taking on.*

* Payments, prices, tips, or any paid transactions. This exchange is free.
* Public sign-up. Access is invite-only.
* "Looking for" posts - posters only offer up what they have
* Production-grade scaling, load handling, and security hardening. This is a class project, not a commercial product.
* Web-app or web ui support
* Multi-language support
* Mobile app
* Delivery, shipping, maps, or route planning. Pickup is coordinated through the message thread and the listing's pickup window.
* Live chat, voice, or video. Messaging is a simple thread for each exchange.
* Recommendation or smart-matching algorithms.
* Formal accessibility certification.

# **3\. Team Members and Roles**

*List each member, their role, and the responsibilities they will primarily own.*

These roles describe who keeps an eye on each area. They are not silos. Everyone writes code, reviews pull requests, and helps where the project needs it. Secondary responsibilities give us backup so work does not stop when one person is busy. Where the team document still has a blank cell, the entry below is a proposed starting point; we will confirm roles at our first meeting.

| Team Member | Role | Primary Responsibilities | Secondary Responsibilities |
| ----- | ----- | ----- | ----- |
| Kennan Kaneshiro | Team Member | Write backend code (Python, SQL, TypeScript) to connect application frontend to backend server and database. | Creating and organizing documentation of the project for maintainable development. |
| Shea Stevens | Database Lead | Database design and administration; schema ownership | Front end system design |
| Matt Ong | QA lead | Test planning, bug tracking | Design contributor and Test tool reviewer |
| Kim Cates | Team Lead and Scrum Master | Runs meetings, keeps the project board current, tracks milestones and deadlines, watches scope, makes sure deliverables ship on time, and coordinates presentations | Documentation and backup front end |
| Jeff Wu | Team Member | Set up and maintain the Docker environment for PostgreSQL, npm scripts for easy, cross-platform local builds, and Github Actions for linting to run on each PR | Write Typescript to implement front end functionality and Python for backend functionality as needed for any User Story |

# **4\. Team Values**

*Choose 4 to 6 values that describe how the team wants to work together.*

* **Talk early and honestly.** If something is confusing, blocked, broken, or bigger than expected, we say so early and help each other figure it out.
* **Accountability**. Given the team's busy schedules, keeping deliverables on schedule helps others keep their requirements on track as well.
* **Flexibility**. On the flip side of the coin, remember that things do come up, and working together as a team to fix issues as they occur is equally important.
* **Share Knowledge.** If you figure something out, or are skilled in a certain concept, help others to achieve a better understanding together.
* **Fairness.** Share workload and support each team member with respect
* **Make room for everyone.** Every teammate should have a real voice in decisions, and we assume good intent.
* **Build work we can demo.** We do not merge code we would be nervous to show. We test, review, and clean up before calling work done.
* **Keep learning.** We are here to get better at React, Python, PostgreSQL, testing, and teamwork. Mistakes are useful when we learn from them.
* **Review the work, not the person.** Feedback should be specific, respectful, and paired with a useful suggestion when possible.

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

* **Keep commitments realistic:** we each take a fair share of work for each milestone, keep our GitHub issues current, and speak up early if there is a problem or something might slip.
* **Get Merge Approval:** Have at least one other team member review your code before merging your code
* **Be Responsive:** Use the discord server as a single point of contact, make sure everyone is on the same page as we work through requirements.
* **Show up and stay in the loop:** attend the weekly meeting and post short Discord updates on the days we agree to. If you cannot make a meeting, say so ahead of time and read the notes afterward.
* **Reply within a reasonable time:** answer Discord messages within 24 hours on weekdays. If a teammate is blocked by your answer, reply the same day when possible. Acknowledge issues assigned to you within 24 hours.
* **Share the useful and less glamorous work:** everyone should get meaningful coding, testing, and documentation tasks. We will rotate chores like note-taking and cleanup.
* **Bring real progress to meetings:** push work in progress before the weekly meeting so discussion and review are based on actual code, not memory.

# **7\. Decision-Making Process**

*Explain how the team will make technical, scope, and scheduling decisions.*

* **Start with consensus:** most of our decisions should come from talking through it in a meeting or on Discord until the team agrees.
* **Technical decisions:** proposed in a GitHub issue or meeting. If the team cannot agree, the lead for that area makes the call and records the reason.
* **Scope decisions:** Proposed in a meeting or GitHub issue. Changes that add or remove features require the whole team to weigh in. Discord messages can be used to raise concepts.
* **Scheduling decisions:** Due to working schedule, we'll most likely be meeting in the evening or on the weekends. We should do our best to review the milestone timeline, and address any deadline risks or blockers to reprioritize if necessary.
* **Tie breakers:** if a vote splits two to two, the Team Lead breaks the tie and records the decision and reason.
* **Record decisions:** important decisions go into the meeting notes so we do not have to rely on memory.

# **8\. Development Workflow**

*Describe how work will be tracked, how branches and pull requests will be used, and what "done" means.*

* **Work tracking:** every task, user story, and bug gets a GitHub issue with an owner and a spot on the Projects board: To Do, In Progress, In Review, or Done.
* **Git Branches:** Work should be done on git branches corresponding to its GitHub Issue
* **Code Reviews:** Code should be reviewed (at least one approval) before it is merged to the main branch.
* **Definition of Done:** Completed functionality after code review of implementation and verification it passes QA tests, merged with main and updated with appropriate commit message
* **Branch protection and naming:** nobody commits directly to main; main is protected. Branches are named like `feature/<issue#>-short-description` or `fix/<issue#>-short-description`.
* **Commit messages:** use clear, present-tense messages that say what changed and why, and include the issue number when possible. Example: `Add over-claim validation to claim service (#42)`.

# **9\. Quality Standards**

*Define your expectations for code quality, testing, documentation, and deployment readiness.*

* **Continuous integration:** a GitHub Actions workflow runs our linters on every pull request, Ruff for the Python code and ESLint for the TypeScript code. These checks must pass before a pull request can merge, which protects main alongside reviews and branch protection.
* **Testing:** Every business rule (claim quantity validation, status transitions, permission checks) must have at least one automated unit test before the feature is considered done.
* **Code coverage:** Aim for meaningful coverage on backend business logic; coverage reports are generated and reviewed at each milestone.
* **Clear set up and transparency:** README, setup instructions, documentation, and comments
* **Code quality:** use a linter and formatter on both stacks, such as Ruff and Black for Python and ESLint and Prettier for TypeScript. Keep functions small, names clear, and commented-out dead code out of main.
* **Manual and UI testing:** write manual test cases for key flows. UI automation with Playwright is a stretch goal.
* **Deployment readiness:** keep seeded demo data and a tested deployment guide so the app can be brought up cleanly from a fresh checkout.

# **10\. Project Artifacts**

*List the project artifacts the team commits to maintaining during the course.*

* **Team and process:** team charter; GitHub repository with README; protected main branch; pinned Discord repo link; project board, issues, and milestones; meeting notes and decision log; retrospective and defect-triage notes.
* **Requirements:** high-level use cases; 25 to 30 user stories with acceptance criteria; domain model; cross-team requirements review packet and report.
* **Code and quality:** manual test cases and GitHub Actions CI workflows.
* **Documentation:** Project documentation that details how the system is designed, features, database schemas, etc. for easy management and future development.
* **Deployment:** Dockerfile and docker-compose configuration, environment variable documentation, and a validated step-by-step deployment guide with seeded demo data.
* **Testing:** Automated unit test suite covering business rules and permission checks; QA packet with manual test cases tied to user stories including edge cases and negative paths.
* **Design:** technical design document with an architecture diagram, components, ERD, data model, key API endpoints, technology stack, risks, and tradeoffs.
* **Release readiness:** a release-readiness document with implemented features and known limitations, presentation decks for inception, R1, R2, and final, and the final QA summary.
* **Individual accountability:** peer evaluations and individual reflections.

Drafts live in Google Drive. Final versions are committed to or linked from the repo.

# **11\. Conflict Resolution and Accountability**

*Explain what the team will do if someone is unresponsive, misses work, or if a conflict arises.*

* **Talk to each other first:** if there is a disagreement or someone is falling behind, the people involved talk directly and respectfully as early as possible.
* **Handle silence or missed work quickly:** if a member goes quiet or misses a commitment, the team reaches out on Discord. If there is no response within two days, the team reassigns the at-risk work to keep the project moving and records it in the meeting log.
* **Separate the problem from the person:** Feedback is about the work, not the individual.
* **Have clear Deliverables:** Ensure task is explicitly defined to prevent miscommunication
* **Bring in the team when needed:** if a direct conversation does not fix the issue, raise it in the weekly meeting so the team can agree on next steps.
* **Escalate repeated or serious issues:** if a problem keeps coming back or cannot be resolved inside the team, we raise it with the instructor. Contributions are visible through GitHub activity and reflected honestly in peer evaluations.

# **12\. Risks and Mitigation**

*Identify likely risks and write how the team will reduce or manage them.*

* **Someone becomes unavailable. Mitigation:** use secondary owners for every area, distribute issues fairly each milestone, and follow the escalation steps in Section 11.
* **Scope creep.** Mitigation: any new feature idea goes through the full team before being added; out-of-scope items are logged but deferred.
* **Integration issues between frontend and backend.** Mitigation: agree on API contracts early and document them in the design doc before implementation begins.
* **The stack has a learning curve.** Some members may be new to React, TypeScript, FastAPI, or PostgreSQL. Mitigation: build a thin end-to-end slice early by R1, pair on hard parts, and use lecture material and office hours.
* **Core business-rule bugs sneak in.** The highest-risk rules are over-claim prevention, status transitions, and no cancellation after pickup. Mitigation: write automated tests for these rules early, cover edge and negative paths, and require review on any pull request that touches them.
* **Setup drifts across machines.** Mitigation: everyone sets up their environment in week 1, we keep setup steps and seeded demo data in the README, and CI catches breakages early.
* **Deadlines get tight.** Mitigation: track milestones on the board, review progress in the weekly meeting, and protect must-have work before nice-to-have work.

# **13\. Milestones and Timeline**

*Capture major checkpoints and target dates.*

| Milestone | Target Date | Owner(s) | Notes |
| ----- | ----- | ----- | ----- |
| Team Formation (repo, board, Discord) | June 1, 2026 | TBD | Teams, repo, and Discord due June 1 |
| Team Charter | June 5, 2026 | TBD | Charter due June 5 |
| Inception Presentation | June 8, 2026 | TBD | Presentations on June 9, approximately 10 minutes total |
| Core Requirements Artifacts | June 8, 2026 | TBD | Core requirements and requirements packet |
| Cross-Team Requirements Review Packet | June 15, 2026 | Shea | Presented June 16 |
| Technical Design Document | July 1, 2026 | TBD | Architecture and components, ERD, API endpoints, tech stack, risks |
| R1 Demo + Presentation | July 6, 2026 | TBD | Presentations on July 7; retrospective and defect triage July 9 |
| Manual Test Cases / QA Packet | July 14, 2026 | Shea | Positive, negative, edge, permission, and workflow tests |
| R2 Demo + Presentation | July 27, 2026 | TBD | Presentations on July 28 |
| Deployment Guide + Release Readiness | August 6, 2026 | TBD | Deployment steps, seeded data, known limitations |
| Final Submission + Final Presentation | August 11, 2026 | TBD | Final demos August 11 to 13; peer evaluation and reflection August 13 |

# **14\. Team Commitment Statement**

We agree to show up, communicate clearly, do our share, and help each other finish this project well. We understand that this course grades both the software we deliver and the engineering process we use to build it. Because of that, we commit to steady collaboration, clear documentation, responsible project management, and technical work we are willing to stand behind.

# **15\. Signatures**

*Each member should acknowledge the charter with a name and date.*

| Name | Signature / Acknowledgment | Date |
| ----- | ----- | ----- |
| Kennan Kaneshiro | Kennan Kaneshiro | 06/05/2026 |
| Shea Stevens | *Shea Stevens* | [Fill in] |
| Matt Ong | Matt Ong | 6/5/2026 |
| Kim Cates | [Fill in] | [Fill in] |
| Jeff Wu | [Fill in] | [Fill in] |
