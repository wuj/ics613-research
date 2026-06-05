# Overview

As part of this course, you will work in teams to implement a web application. As this is a class project, the web application is not expected to be production-ready, released to, or evaluated with real users. Instead, the objective is to practice software engineering concepts and apply the knowledge you've gained throughout the course.

You will learn important skills such as teamwork, communication, project management, and coding standards. Each team will be responsible for the full development lifecycle, including requirements gathering, design, implementation, testing, and documentation.

The high-level description for each project is provided. You are expected to expand on this description and define the specific features, functionality, and user interfaces of your application. Each team should create detailed user stories that outline different user roles and the tasks each role will perform within the application. For each requirement, you would also need to design comprehensive test cases that include not only the default behavior but also edge cases. You should also create a technical architecture/design document that outlines the structure and components of your application. This document should also include diagrams, such as UML diagrams, database design, technical constraints, and assumptions.

At a minimum, the technology stack should include:

- React and Typescript for the front end
- Python (FastAPI, SQLAlchemy, and Pydantic) for the service/backend
- PostgreSQL for the database

You are encouraged to use open-source libraries and frameworks to enhance functionality and streamline development, such as UI component libraries, authentication and user management libraries, and unit testing frameworks.

You are expected to use an Integrated Development Environment (IDE), such as Visual Studio Code, for development. You can also utilize your hawaii.edu email to obtain a license for the JetBrains IDEs and development tools for free.

Please note that this is not a UI/UX/Design class, so you should not spend excessive effort on implementing a sophisticated user interface. The richness/sophistication of the UI will not be evaluated; instead, focus on the functionality and underlying code structure. At a minimum, you should ensure that the web application is responsive (i.e., automatically adapts its layout and functionality to fit any device).

It is expected that each team member has the development environment set up and functioning on their workstation/laptop.

You must utilize GitHub as your version control system. One team member can create the repository and invite the rest of the team to collaborate. It is essential that you leverage GitHub's features effectively, including branching for managing different features or fixes, pull requests for code reviews and collaborative development, issues for tracking tasks and bugs, Projects for project management, milestones to track progress toward key deadlines, and wiki for project documentation. You are also encouraged to use GitHub Actions for continuous integration and deployment (CI/CD).

Your instructor will create a channel on the class Discord server for your project. You are expected to use this channel for all project-related communication among the team. You should also create a pinned message that includes the URL of the GitHub repository.

---

## Project: Local Produce Exchange

For this semester-long group project, your team will design, develop, and deploy a responsive web application that reduces food waste by enabling an invite-only exchange of surplus produce and other food items within a trusted private community.

Users join only via invitation, create a profile, and can post listings for items they want to share with required details: description, category (e.g., fruits, vegetables, baked goods, dairy), photos (optional), quantity available, dietary/allergen tags (e.g., vegan, contains nuts, gluten-free), and an available pickup window. Members can browse/search listings and submit a claim request for a specified quantity. Each request follows the sequence REQUESTED -> APPROVED -> PICKED_UP -> COMPLETED, with REQUESTED -> DENIED as a terminal rejection path, and CANCELLED allowed only from REQUESTED or APPROVED (cancellation after pickup is not allowed). The system must prevent conflicting claims by ensuring that approved quantities never exceed the remaining quantity. Each exchange includes a private message thread for coordination and generates notifications for status changes and pickup reminders (if implemented). After completion, both sides can leave a short rating/review.

The application must include user profiles showing a member's active listings, incoming/outgoing requests, and exchange history, plus a dashboard to manage actions by status (approve/deny, cancel, picked up, completed). An admin role can suspend users, deactivate listings (hide from browsing without deleting audit history), and generate basic reports.

The deliverable is a deployed web app with a clear README, seeded demo data, and automated tests covering permissions and the main end-to-end flow. Your team is expected to apply software engineering practices from the course (requirements, design, version control, code reviews, testing, and iterative delivery) through incremental milestones aligned with the course schedule.

Remember, *the goal is not to build a commercial-grade product, but to gain hands-on experience with the full software development lifecycle while working collaboratively as a team*.

### Sample User Stories

**User Story #1:**

As a claimant, I want to submit a claim for a specified quantity of a listing so that I can request only the amount I need.

**Acceptance Criteria:**

**Scenario 1: Valid claim request**
- Given a listing is active
- And the listing has 10 units available
- When the claimant requests 3 units
- Then a claim is created with status REQUESTED
- And the requested quantity is 3

**Scenario 2: Claim quantity exceeds available quantity**
- Given a listing is active
- And the listing has 10 units available
- When the claimant requests 12 units
- Then the system rejects the claim
- And no claim is created

**Scenario 3: Claim quantity must be positive**
- Given a listing is active
- When the claimant requests 0 units
- Then the system rejects the claim
- And displays a validation error

**User Story #2:**

As a claimant or poster after completion, I want to leave a short rating and review of the other party, so that the community can build trust.

**Acceptance Criteria:**

**Scenario 1: Claimant leaves a review after completion**
- Given an exchange has status COMPLETED
- And the logged-in user is the claimant for that exchange
- And the claimant has not already reviewed the poster
- When the claimant submits a rating and short review for the poster
- Then the review is saved
- And the review is associated with that completed exchange
- And the poster can view the review

**Scenario 2: Poster leaves a review after completion**
- Given an exchange has status COMPLETED
- And the logged-in user is the poster for that exchange
- And the poster has not already reviewed the claimant
- When the poster submits a rating and short review for the claimant
- Then the review is saved
- And the review is associated with that completed exchange
- And the claimant can view the review

**Scenario 3: Review is not allowed before completion**
- Given an exchange has status REQUESTED, APPROVED, PICKED_UP, DENIED, or CANCELLED
- When either party attempts to submit a review
- Then the system rejects the review
- And no review is saved

**Scenario 4: Non-participant cannot review**
- Given an exchange has status COMPLETED
- And the logged-in user is not the poster or claimant for that exchange
- When the user attempts to submit a review
- Then the system denies access
- And no review is saved

### Sample Test Cases

| Field | Example |
| --- | --- |
| Test case ID | TC-001 |
| Related story | Leave rating and review after completion |
| Precondition | Exchange #42 exists with status COMPLETED; user A is the claimant; user B is the poster |
| Test data | Rating: 5; Review text: "Easy pickup and fresh produce." |
| Steps | 1. Log in as user A. 2. Open exchange #42. 3. Click "Leave Review." 4. Enter rating and review text. 5. Submit. |
| Expected result | Review is saved, linked to exchange #42, and visible to the poster |
| Actual result | *Filled in during testing* |
| Pass/fail | *Filled in during testing* |

| Field | Example |
| --- | --- |
| Test case ID | TC-002 |
| Related story | Invite-only registration |
| Precondition | Valid unused invite token exists |
| Test data | First Name: John; Last Name: Smith, etc. |
| Steps | 1. Open registration page. 2. Enter account information and valid token. 3. Submit. |
| Expected result | Account is created. Token is marked as used. |
| Actual result | *Filled in during testing* |
| Pass/fail | *Filled in during testing* |

---

## Project Deliverables

### 1) Team + Process Setup

**A. Team charter**

**B. Repository setup**
- Repository, issue, project board, etc., setup
- README stub
- Configure the repository to prevent commits directly to main

---

### 2) Requirements

**A. Core Requirements**
- High-level use cases
- 25 to 30 user stories (including acceptance criteria)
  - The 25 to 30 user stories should represent about 30 distinct new functionalities. Do not reach the count by splitting one core feature across roles, goals, scenarios, or edge cases.
  - You can still have separate user stories for the different goals of each user type (for example, "owner lists a tool" and "borrower borrows a tool").
  - Edge cases are captured as scenarios within a story's acceptance criteria, not as separate user stories.
  - Permissions are also captured as acceptance criteria scenarios (for example, when an owner lists a tool, a non-owner cannot delist it).
- Domain model
  - Repository synthesis note, added after the original requirements text: The
    original deliverable list names "Domain model" but does not define the
    expected format in this section. The clearest course source is the Week 02a
    Requirements Engineering lecture, which defines a domain model as a
    conceptual representation of the key entities, attributes, and
    relationships in the problem domain. The same lecture says the model
    describes what exists in the real-world domain the system must support, not
    how the software will be built.
  - Based on Week02a_RequirementsEngineering.pdf, pages 14 to 16, the expected
    artifact should be a UML-style domain class diagram. It should include:
    domain entities, plain attribute names, named relationships, and
    multiplicity/cardinality on relationships.
  - The domain model should not be treated as the database schema or ERD.
    Week02a, page 14, says the domain model is not a database schema, although
    the two artifacts can be similar. This distinction is also supported later
    in this requirements document, where the mid-semester presentation asks for
    an overview of both the domain model and the ER diagram.
  - Keep implementation details out of the domain model. Week02a, page 15,
    says domain-model attributes should not include visibility modifiers or
    types because those are design decisions. By the same reasoning, avoid SQL
    types, primary keys, foreign keys, indexes, and implementation methods
    unless a concept is truly part of the problem domain.
  - Supporting repo evidence: requirements.md line 272 asks for both the domain
    model and ER diagram in the mid-semester presentation; requirements.md line
    335 says the domain model is not required for the review packet but is a
    nice-to-include item; meeting-notes/2026-06-02-meeting-notes.md line 42
    summarizes the domain model as a diagram of the key things in the problem
    area and how they relate to each other.

**B. Cross-Team Requirements Review Packet**

**C. Cross-Team Requirements Review Report**

---

### 3) Technical Design

**A. Lightweight design doc**
- Architecture diagram + components
- Technology stack (including libraries and frameworks)
- Data model (ERD)
- Key API endpoints
- Risks/tradeoffs listed

**B. Code quality tools**
- Tools that will be utilized
- When and where will they be executed

---

### 4) Implementation
- Use of GitHub as the code repository
- Branch-based development and code reviews before merging to main
- Automated unit tests for business rules
- Use of appropriate commit messages
- Use of code quality tools

---

### 5) Testing

**A. Test cases for manual testing**
- Create the key test cases
- Nice to have is using a tool like Playwright to automate UI testing

---

### 6) Deployment

**A. Deployment guide**
- Create (and validate) a step-by-step guide for deploying the web application
- Document detailing the implemented features, known limitations, and risks

---

### 7) Communication Deliverables

**A. In-class presentations and demos**
- Clear walkthrough covering required scenarios
- Ability to answer questions posed by the instructor (and attendees)

**B. Peer evaluations**
- Evaluations for each team member

---

## Grade Distribution

| Project component | Due / milestone | Weight | Main grading focus |
| --- | --- | --- | --- |
| 1. Team + process setup | 6/1 and 6/4 | 3% | Team charter, GitHub repo, project board, README stub, branch protection, Discord/repo link |
| 2. Inception presentation + requirements packet | 6/8 | 8% | Project scope, roles/personas, core use cases, 25-30 user stories (distinct functionalities), acceptance criteria (including edge cases and permissions), assumptions, risks |
| 3. Cross-team requirements review | 6/15 | 4% | Quality of feedback given to another team: ambiguity, missing cases, workflow gaps, scope risks |
| 4. Technical design document | 7/1 | 7% | Architecture diagram, components, ERD/data model, API endpoints, tech stack, code quality tools, tradeoffs |
| 5. R1 demo + presentation + code walkthrough | 7/6-7/9 | 7% | First working vertical slice, evidence of implementation progress, ability to explain code, retrospective/defect triage |
| 6. Manual test cases / QA packet | 7/14 | 4% | Test cases tied to user stories, positive and negative paths, edge cases, permission/workflow tests |
| 7. R2 demo + presentation + code walkthrough | 7/27 | 7% | Broader feature completion, improved quality from R1, defect resolution, updated risks/scope |
| 8. Deployment guide + release readiness | 8/6 | 3% | Clear deployment steps, seeded demo data, known limitations, validated setup instructions |
| 9. Final submission + final presentation | 8/11-8/13 | 9% | End-to-end demo, completed requirements, final codebase, README, limitations, QA summary, lessons learned |
| 10. Individual contribution and engagement evidence | | 8% | GitHub activity, issue ownership, code reviews, testing/documentation work, presentation accountability, peer evaluation, reflection |

### Individual Contribution Breakdown

| Individual contribution category | Weight |
| --- | --- |
| GitHub contribution evidence: meaningful PRs, commits, reviewed PRs, tests, documentation | 3% |
| Issue ownership and project management: assigned issues, project board updates, meeting notes, milestone tracking | 1.5% |
| Code review / testing / QA contribution | 1.5% |
| Presentation, demo participation, and ability to answer questions | 1% |
| Peer evaluation + individual reflection | 1% |

---

## Project Presentations

*Note: All team members should collaborate in preparing the presentation. Each team member should present a similar number of slides.*

### Project Initiation

*At a minimum, the presentation should contain the following.*
- Project objectives
  - Outline the goals of the project
  - What value will this project bring to the client/end users?
- Project scope
  - Clearly define what is included in the project scope and what is not
- User roles/personas
  - Identify the different user roles and personas involved
- Key use cases and user stories
- Acceptance criteria
- Project management overview
  - Team member roles and responsibilities
  - Recurring meeting schedule and plan for tracking meeting minutes
  - Task management and tracking
- Key risks and mitigation strategies
- Project Artifacts
  - List the documents and tools the team will create (e.g., requirements specification, design specification, test cases, deployment plan, etc.)
- Quality Assurance Plan
  - Outline the approach for quality assurance, defect tracking, and code quality
- Milestones
  - Define what will be delivered at each milestone
- Next steps

### Mid-Semester - Presentation + Live Demo

*At a minimum, the presentation should contain the following.*
- Overview of the domain model and the ER diagram
- Major accomplishments
  - Include screenshots
  - Include brief details about the technical implementation
- Major changes, if any, to the requirements
- Changes, if any, to the project management approach since the initiation
- Updated key risks and mitigation strategies, if any
  - Were there any risks identified in Project Initiation that materialized? How were they addressed?
- Quality assurance metrics
  - Defects reported, fixed, open, etc.
  - Code quality metrics, unit test coverage
- Milestones
  - Missed/Posponted milestones
    - Why?
  - Upcoming milestones
    - What will be delivered at these milestones?
- Next Steps

In addition to demonstrating key functionality in the live demo, teams should be prepared to conduct a code walkthrough and explain key components of the codebase. The instructor and audience will ask questions regarding functionality and the codebase.

There will be two mid-semester demos - R1 and R2

### Final Presentation - Presentation + Live Demo

*At a minimum, the presentation should contain the following.*
- Project objectives
  - Provide a brief overview of the project's goals
  - What original project goals/features were not achieved and why?
- Technical solution overview
- Major Accomplishments
  - Highlight key achievements, including any requirements that were not implemented or were partially done, along with the rationale for these decisions.
  - Include key screenshots
- Quality Assurance Metrics (Overall)
  - Provide an overall summary of defects reported, fixed, and currently open
- Project Management and Delivery
  - Overview of the software process the team followed, including the use of AI in development.
- Reflections
  - What went right?
  - What went wrong?
  - What could have been done differently?
  - Discuss both technical and soft skills acquired or improved by team members
- Acknowledgments (if any)

In addition to demonstrating key functionality in the live demo, teams should be prepared to conduct a code walkthrough and explain key components of the codebase. The instructor and audience will ask questions regarding functionality and the codebase.

---

## Cross-Team Requirements Peer Review

Each team will review the other team's project requirements and provide structured feedback. The goal is to identify ambiguity, missing requirements, unrealistic scope, inconsistent workflows, unclear user roles, and missing edge cases before implementation begins.

The reviewing team will act as informed stakeholders and peer software engineers. Their responsibility is not to design the solution but to offer a fresh perspective before the other team commits to design and implementation decisions.

### Artifacts for review

Each team should submit a Requirements Review Packet containing (per page 16 of the Course Project.pdf):

1. User stories
2. Acceptance criteria
3. User roles/personas
4. Open questions and assumptions
5. Known risks

The domain model is not required for the packet, but it is a nice-to-include item.

### Conducting the review

The following are suggestions to assist the reviewing team:

1. User stories - do they describe behavior clearly enough to facilitate testing?
2. Acceptance criteria - are they specific and complete enough to determine when the story is done?
3. User roles/personas - have all relevant user roles (member, owner/poster, admin, guest) been accounted for?
4. Edge cases - what scenarios have not been planned for?
5. Requirements consistency - are there contradictions between stories? Does any story depend on elements not included in the backlog?

### Review feedback

The Requirements Review Report, typically 2-4 pages, should include the following:

1. Summary - an overall impression of the requirements
2. Strengths - Clear, useful, or well-scoped elements
3. Ambiguities - requirements that could be interpreted in multiple ways
4. Missing cases - edge cases, failure cases, permissions, or workflow gaps
5. Scope risks - features that seem too large or technically risky

### Presentation and class discussion

Each team will present its Requirements Review Report in a 10-minute presentation, followed by a brief Q&A session. During this discussion, the team whose requirements are being reviewed should:
- Be prepared to clarify any aspects of their requirements that may have been questioned during the review.
- Take notes on feedback and suggestions for subsequent revisions of their requirements.

---

## Team Charter

**Template:**
https://docs.google.com/document/d/1O_wL98pSWUaWf7yBahiy3-LGO1QjNDpl/edit?usp=sharing&ouid=103020945264628236703&rtpof=true&sd=true

**Sample:**
https://docs.google.com/document/d/1lLudKSwsn1fTSAduMEkGIOIS7VJFAUCk/edit?usp=sharing&ouid=103020945264628236703&rtpof=true&sd=true

---

## Clarifications (Instructor Q&A)

This section preserves clarifying questions students asked and the instructor's answers. The relevant details have also been folded into the sections above.

**Q1: 25-30 user stories requirement (Rion Sawabe).** Should the 25 to 30 user stories represent 30 new functionalities, or can they be created by breaking down the core functionality into different user roles, goals, scenarios, and edge cases?

**A1 (Anthony Peruma).** They should represent 30 new functionalities. Edge cases should be part of the acceptance criteria scenarios. Permissions should also be captured under acceptance criteria scenarios (for example, if an owner lists a tool, a non-owner cannot delist it). However, you can have user stories for the different goals of each user type, for example: (1) owner lists a tool, (2) borrower borrows a tool.
