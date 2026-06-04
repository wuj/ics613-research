# User Story Template (Local Produce Exchange)

This template is the standard way our team writes user stories for the Local
Produce Exchange project. A user story is "a short, user-centric description of
a requirement" (Week 02 Requirements Engineering lecture, "User stories" slide,
p.11). Each story names a user, the goal that user wants, and the benefit they
get. Each story also lists acceptance criteria, which are the testable
conditions that tell us when the story is done.

To use this template: copy the blank form in Section 3 for each new story, fill
in every field, run the INVEST self-check in Section 4, then open a GitHub Issue
as described in Section 5. Section 7 shows one fully completed example you can
copy from.

---

## 1. How to write a good user story

### The format

Write the story in one sentence using this exact format (Week 02 lecture,
"User stories" slide, p.11):

```
As a [role], I want [goal] so that [benefit].
```

- **role** is the type of user (for example, claimant, poster, member, admin).
- **goal** is what that user wants to do.
- **benefit** is why it matters to them.

The "so that" clause is required, not optional. The lecture is explicit: the
"so that" clause "tells the team why the feature matters and helps prevent
unnecessary features" (Week 02 lecture, "User stories" slide, p.11). If you
cannot state a real benefit, the feature may not be needed.

### What makes a requirement "good"

The lecture lists six qualities every requirement should have (Week 02 lecture,
"Writing good requirements" slide, p.17). Check your story against all six:

1. **Correct** -> it accurately reflects a real stakeholder need.
2. **Unambiguous** -> it is open to only one interpretation.
3. **Complete** -> it contains all the information needed to build and test it.
4. **Consistent** -> it does not contradict any other requirement.
5. **Verifiable** -> it can be tested or demonstrated to confirm it is met.
6. **Traceable** -> it can be linked back to its origin and forward to design
   and tests.

### One story = one distinct functionality

Per the instructor Q&A for this project (Answer A1), each user story should
represent one distinct functionality. Do not write separate stories for edge
cases or for permission rules. Instead, capture those as acceptance-criteria
scenarios inside the relevant story. For example, "a non-owner cannot delist a
listing" is a permission scenario, not its own story.

---

## 2. Acceptance criteria: what to cover

Acceptance criteria are the "specific, verifiable conditions that must be true
for the story to be complete" (Week 02 lecture, "Acceptance Criteria" slide,
p.12). Write them as Gherkin scenarios using Given / And / When / Then. The
lecture says each story's criteria should include these (same slide, p.12):

- **Specific behavior** -> what the system actually does.
- **Observable result** -> something you can see or check.
- **Normal path** -> the main success case.
- **Edge/error paths** -> what happens with bad or unusual input.
- **Permission rules** -> who is and is not allowed to do this.

Each scenario should map to at least one test.

---

## 3. The template (blank form)

Copy this block for each new story and fill in every field.

```
Story ID: US-000
Title: <short summary of the story>

Story:
As a <role>, I want <goal> so that <benefit>.

Priority: <high | medium | low>
Actor: <member | poster | claimant | admin | ...>
Milestone: <which milestone / sprint this belongs to>

Acceptance Criteria:

Scenario 1 (normal / valid path):
  Given <starting condition>
  And <another condition, if needed>
  When <the action the user takes>
  Then <the expected result>
  And <another result, if needed>

Scenario 2 (edge / error path):
  Given <starting condition>
  When <the action with bad or unusual input>
  Then <the expected rejection or error>

Scenario 3 (permission rule):
  Given <starting condition>
  And <the user is not allowed to do this>
  When <the user attempts the action>
  Then <access is denied and nothing changes>

Notes / open questions / assumptions:
- <anything the team still needs to decide>
```

**Scenario coverage note.** Always include the normal path. Add an edge/error
scenario when the action has bad-input or boundary cases, and add a permission
scenario when the action is restricted by role. Add only the extra scenarios a
story actually needs. If the story touches claim status, quantity,
cancellation, pickup, completion, notification, or review behavior, add the
specific workflow or conflict scenarios that cover those rules (for example,
"cancellation is not allowed after pickup"). Do not pad a story with scenarios
that do not apply.

---

## 4. INVEST self-check

Before you mark a story ready, check it against the six INVEST criteria
(Week 02 lecture, "INVEST criteria for user stories" slide, p.13):

- [ ] **Independent** -> the story can be developed and delivered on its own,
      without depending on other stories.
- [ ] **Negotiable** -> the details are open to discussion; the story is a
      placeholder for conversation, not a rigid contract.
- [ ] **Valuable** -> the story delivers clear value to the user or customer,
      not just technical tasks.
- [ ] **Estimable** -> the team can reasonably estimate the effort to build it.
- [ ] **Small** -> the story is small enough to finish within a single sprint
      iteration.
- [ ] **Testable** -> clear acceptance criteria exist so the team knows when the
      story is done.

---

## 5. GitHub Issue mapping

We track stories in GitHub (Week 02 lecture, "Managing Requirements with
GitHub" slide, p.25). Each user story becomes one GitHub Issue that is tracked,
assigned, linked to code, and closed when the story is done.

Use these labels on the Issue:

- **Priority**: high, medium, or low.
- **Type**: story (use the GitHub `Type: story` label on the Issue itself; you
  do not need a Type field in the story body, since every item from this
  template is a story).
- **Actor**: who the story is for. The roles our requirements name today are
  member, poster, claimant, and admin. As our role model grows, the team may
  add guest, invited-user, and suspended-user (the project is invite-only and
  admins can suspend users).

Assign each Issue to a **Milestone** so it shows up in the right sprint, and add
it to the GitHub Project board so it moves across the Kanban view as its status
changes.

We keep the story body minimal: record only Priority, Actor, and Milestone in
the body (as shown in the blank form). Type stays on the Issue as a label.

---

## 6. Definition of Ready and Definition of Done

These two checklists are team process checks. They are different from acceptance
criteria: acceptance criteria are conditions specific to one story, while
Definition of Ready and Definition of Done apply to how the team handles every
story.

**Definition of Ready** (a story is ready to be worked on when):

- [ ] It has a clear role, goal, and benefit in the one-sentence format.
- [ ] It has acceptance criteria written as Gherkin scenarios.
- [ ] It passes the INVEST self-check.
- [ ] The team can estimate the effort to build it.

**Definition of Done** (a story is done when):

- [ ] All acceptance-criteria scenarios pass.
- [ ] Tests are written where appropriate.
- [ ] The code has been reviewed.
- [ ] The work is merged through the team's normal pull request process.

---

## 7. Worked example

This example is modeled on Sample User Story #1 in the project requirements (a
claimant submits a claim for a quantity). It reuses that story's three
validation scenarios and adds one permission scenario. The permission scenario
is an extension required by the instructor Q&A rule (Answer A1) that permission
rules belong in acceptance criteria, not in a separate story.

```
Story ID: US-001
Title: Claimant submits a claim for a quantity of a listing

Story:
As a claimant, I want to submit a claim for a specified quantity of a listing
so that I can request only the amount I need.

Priority: high
Actor: claimant
Milestone: Sprint 1 - Core exchange flow

Acceptance Criteria:

Scenario 1 (normal / valid path):
  Given a listing is active
  And the listing has 10 units available
  When the claimant requests 3 units
  Then a claim is created with status REQUESTED
  And the requested quantity is 3

Scenario 2 (edge / error path - quantity exceeds available):
  Given a listing is active
  And the listing has 10 units available
  When the claimant requests 12 units
  Then the system rejects the claim
  And no claim is created

Scenario 3 (edge / error path - quantity must be positive):
  Given a listing is active
  When the claimant requests 0 units
  Then the system rejects the claim
  And displays a validation error

Scenario 4 (permission rule - added per instructor Q&A A1):
  Given a listing is active
  And the user submitting the request is suspended
  When the suspended user attempts to submit a claim
  Then the system denies access
  And no claim is created

Notes / open questions / assumptions:
- Assumes a "suspended" user state exists in the role model. Confirm with the
  team how suspension is represented before building Scenario 4.
```
