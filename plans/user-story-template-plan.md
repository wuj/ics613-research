# Plan: User Story Template for ICS 613 Project

## Context

The team must produce 25 to 30 user stories with acceptance criteria for the
Local Produce Exchange project (Deliverable 2, due 6/8, worth 8% of the grade).
Per the instructor Q&A in `requirements.md`, these stories must represent about
30 distinct functionalities, with edge cases and permissions captured as
acceptance-criteria scenarios rather than as separate stories. To keep every
team member writing stories the same way, we will create a single reusable
user story template as a Markdown artifact. The template must match how the
professor (Anthony Peruma) teaches user stories in the Week 02 Requirements
Engineering lecture, since that is what the deliverable will be graded against.

## Goal

Create `artifacts/user-story-template.md`: a guide, a blank fill-in form, and
one worked example, all aligned to the lecture material and the project's
Gherkin-style acceptance criteria. The template should be minimum viable:
include only the fields a team member needs to write a clear, testable user
story that satisfies the lecture guidance and project requirements. Avoid extra
metadata fields unless they directly support grading, GitHub issue tracking, or
acceptance testing.

## Source material (prioritized: professor's lectures first)

Primary source, `ics613-research/lectures/Week02a_RequirementsEngineering.pdf`:
- "User stories" slide: format `As a [role], I want [goal] so that [benefit]`;
  a user story is a short, user-centric description of a requirement; the
  "so that" clause is NOT optional (it explains why the feature matters and
  helps prevent unnecessary features); acceptance criteria define when a story
  is complete (testable conditions).
- "Acceptance Criteria" slide: every story needs acceptance criteria that are
  specific and verifiable; each scenario maps to at least one test; what to
  include = Specific behavior, Observable result, Normal path, Edge/error
  paths, Permission rules.
- "INVEST criteria" slide: Independent, Negotiable, Valuable, Estimable, Small,
  Testable (with the lecture's "what it means" wording).
- "Writing good requirements" slide: Correct, Unambiguous, Complete,
  Consistent, Verifiable, Traceable.
- "Managing Requirements with GitHub" slide: each user story becomes a GitHub
  Issue; labels for Priority (high/medium/low), Type (story/bug/task), Actor
  (member/owner/etc.); use Milestones.

Project source, `ics613-research/requirements/requirements.md`:
- Sample User Stories #1 and #2 show the expected acceptance-criteria style:
  Gherkin scenarios using Given / And / When / Then, multiple scenarios per
  story. Sample User Story #1 shows valid-path and validation-failure
  scenarios. Sample User Story #2 shows valid-path, workflow-status, and
  permission scenarios.
- Instructor Q&A (A1): 30 distinct functionalities; edge cases and permissions
  belong in acceptance-criteria scenarios, not as separate stories.

Supplementary (industry standard, only where it fills a gap the user approved):
- Definition of Ready / Definition of Done checklists.

## File to create

`C:\Users\jeffr\MCS\ics613\artifacts\user-story-template.md` (the `artifacts/`
folder does not exist yet and will be created).

## Template structure

1. **Title + short intro.** One paragraph: what a user story is (lecture
   definition) and how to use this template. Cite the Week 02 lecture.

2. **Section: How to write a good user story (the guide).** Plain-language
   explanation, each point tied to the lecture:
   - The `As a [role], I want [goal] so that [benefit]` format and why the
     "so that" clause is required.
   - The six "good requirement" qualities (Correct, Unambiguous, Complete,
     Consistent, Verifiable, Traceable).
   - The rule from the instructor Q&A: one story = one distinct
     functionality; edge cases and permissions go in the scenarios.

3. **Section: The template (blank form).** A minimum viable fill-in block with
   these fields only:
   - Story ID (e.g., US-001)
   - Title (short summary)
   - Role / Goal / Benefit statement (the one-sentence format)
   - Priority, Actor, Milestone (maps to GitHub Issue labels; Type is assumed
     to be `story` for this template)
   - Acceptance Criteria as Gherkin scenarios. Provide labeled placeholders for
      at least: Scenario 1 (normal/valid path), Scenario 2 (edge or error path),
      Scenario 3 (permission rule), each using Given / And / When / Then. A note
     reminds the writer to cover normal, edge/error, and permission paths. When
     the story involves claim status, quantity, cancellation, pickup, completion,
     notification, or review behavior, the writer should add only the extra
     scenarios needed to cover those workflow or conflict rules.
   - Notes / open questions / assumptions.

4. **Section: INVEST self-check.** A Markdown checkbox list of the six INVEST
   criteria with the lecture's one-line meaning for each, to run before marking
   a story ready.

5. **Section: GitHub Issue mapping.** Short guidance: each story becomes one
   Issue; suggested label scheme (Priority high/medium/low, Type story, Actor
   guest/invited-user/member/poster/claimant/admin/suspended-user); assign to a
   Milestone. Keep the template field list minimal by recording only Priority,
   Actor, and Milestone in the story body; use the GitHub `Type: story` label on
   the issue itself.

6. **Section: Definition of Ready / Definition of Done.** Two short
   checkbox lists (industry-standard supplement the user approved). Make clear
   that acceptance criteria are story-specific conditions, while Definition of
   Ready and Definition of Done are team process checks. Ready = story has
   role/goal/benefit, acceptance criteria, INVEST passes, and can be estimated.
   Done = acceptance criteria pass, tests are written where appropriate, code is
   reviewed, and work is merged through the team's normal PR process.

7. **Section: Worked example.** One fully filled-in story using the Local
   Produce Exchange domain, modeled on Sample User Story #1 in
   `requirements.md` (claimant submits a claim for a quantity), but extended
   with one permission scenario so the example demonstrates the project rule
   that permissions belong in acceptance criteria. The example should show the
   blank form completed end to end with three or four Gherkin scenarios: valid
   claim, quantity exceeds available, quantity must be positive, and unauthorized
   user cannot submit a claim if that rule fits the team's role model.

## Formatting constraints (from CLAUDE.md)

- ASCII-only, UTF-8. No em/en dashes, curly quotes, ellipsis characters, or
  emojis. Straight quotes and apostrophes only. Hyphens only for legitimate
  hyphenation.
- Plain-spoken, layman-friendly prose throughout.
- Cite the specific lecture and slide for course-derived guidance (e.g.,
  "Week 02 Requirements Engineering lecture, 'User stories' slide").

## Verification

- Open the rendered Markdown and confirm all seven sections are present and the
  headings nest correctly.
- Check the worked example against Sample User Story #1 in `requirements.md`:
  same role (claimant), same Gherkin structure, and same validation-failure
  ideas. Confirm the added permission scenario is clearly marked as an extension
  required by the instructor Q&A rule about permissions.
- Confirm the blank form's fields line up with the GitHub Issue label scheme
  from the lecture while staying minimal (Priority, Actor, Milestone in the
  template; Type story as a GitHub label).
- Use the template to draft three quick trial stories: invite-only registration,
  claim approval, and admin user suspension. Confirm that each story has a clear
  role, goal, benefit, normal scenario, edge/error scenario where relevant, and
  permission scenario where relevant.
- Scan the whole file for non-ASCII characters (no dashes, curly quotes, or
  emojis) to satisfy the CLAUDE.md output rule.
