# High-level Use Case Template

Use this template for the "High-level use cases" deliverable listed under Core
Requirements (requirements.md:147-154). Copy the block in Part 2 for each use
case. Part 4 is a worked example. Part 5 is a checklist you run before
submitting.

## Part 0: How this template is grounded in the professor's materials

Honesty first, so you know what is required versus what is standard practice:

- The requirements document asks for "High-level use cases" as a Core
  Requirement (requirements.md:148) and lists "Key use cases and user stories"
  as presentation content (requirements.md:254). The grading focus names "core
  use cases" (requirements.md:218).
- The Week 02 lecture names use cases as one of the ways you write down
  requirements: "Document requirements as user stories, use cases, and
  acceptance criteria" (Week02a, p.2).
- What the professor's documents do NOT provide: a worked use case example or a
  fixed list of use case fields. The only worked examples in the requirements
  document are user stories and test cases (requirements.md:41-128).

So the field structure below is the standard, widely used use case format. The
lecture points to that standard in its Further Reading (UML Distilled and the
SWEBOK Requirements Knowledge Area, Week02a, p.28). Everything else in this
template is pulled straight from the professor's materials:

- The word "high-level" is taken literally. These use cases stay brief. Deep
  detail (exact inputs, every branch, validation messages) belongs in the user
  stories and their acceptance criteria, which are the next deliverable
  (requirements.md:149; Week02a, p.11-12).
- Actors are the project's own roles and the domain is the project's own domain.
- The writing guidance comes from the lecture's domain modeling, good-
  requirements, and pitfalls slides (Week02a, p.14-15, p.17, p.18, p.27).

## Part 1: Core ideas, mapped to the lecture

- An actor is a role that interacts with the system to reach a goal. Actors come
  from your stakeholder and role analysis (Week02a, p.9) and from the nouns and
  roles in your domain model (Week02a, p.15). For the Local Produce Exchange the
  roles named in the requirements are member, poster, claimant, and admin, plus
  guest for anyone not yet signed in (requirements.md:33-35, 341).
- A use case is one complete goal an actor achieves with the system, from start
  to finish. Name it with a verb phrase, the way the lecture derives behavior
  from verbs (Week02a, p.15). Good: "Claim produce from a listing." Weak:
  "Claim screen."
- A use case describes what the system does, not how it is built or how the
  screen looks (Week02a, p.18). "Use a dropdown menu" is a solution, not a use
  case step (Week02a, p.27).
- Each high-level use case later breaks down into several user stories. Record
  that link so your requirements stay traceable, which the lecture calls forward
  traceability from requirement to design to test (Week02a, p.21).

## Part 2: The use case template

Copy this whole block for each use case. Keep the flows short. If you find
yourself writing long, detailed branches, that detail belongs in the user
stories, not here.

```
### Use Case UC-<number>: <verb phrase naming the goal>

- Primary actor: <the role that wants this goal, for example Claimant>
- Supporting actors: <other roles or the system involved; otherwise "None">
- Goal: <one plain sentence stating what the actor wants to accomplish>
- Preconditions: <what must already be true before this can start>
- Trigger: <the event or action that starts this use case>
- Main success flow:
  1. <actor does something>
  2. <system responds>
  3. <continue, in plain steps, until the goal is reached>
- Alternate and exception flows:
  - <name a branch, then what happens; for example "Requested amount exceeds
    what remains: system rejects the request and creates no claim">
  - <add more as needed>
- Postconditions: <what is true after a successful run>
- Related user stories: <the US-<number> items that will detail this use case,
  or "TBD" until they are written>
```

How to fill in each field:

- Use Case ID and name. Number them UC-01, UC-02, and so on, so user stories,
  test cases, and GitHub issues can point back to them (Week02a, p.21). The name
  is a verb phrase stating the goal.
- Primary actor. One role, and it must be a role your project actually defines
  so reviewers can check that every role is covered (requirements.md:341).
- Goal. One sentence. This is the "what," stated plainly (Week02a, p.18).
- Preconditions. The state the system must be in before the use case runs. For
  example, "the claimant is a registered, active member" and "the listing is
  active."
- Trigger. The single event that kicks it off.
- Main success flow. A short numbered list of the normal, everything-works path.
  Alternate steps between actor and system. Keep it high level.
- Alternate and exception flows. Brief bullets naming the important branches:
  invalid input, a rule that blocks the action, or a permission that is missing.
  You do not expand each one here. Each becomes a scenario in the matching user
  story's acceptance criteria, where the lecture says edge and error paths and
  permission rules belong (Week02a, p.12; requirements.md:152-153).
- Postconditions. What is true once the goal is reached.
- Related user stories. The link forward to the detailed deliverable.

## Part 3: Quality bar and the use case diagram

### Write each use case to the lecture's quality bar

Check every use case against the marks of a good requirement (Week02a, p.17):

- Correct: it reflects a real need from the project description.
- Unambiguous: open to only one reading. Avoid vague words like "fast" or
  "user-friendly," which the lecture flags as a pitfall (Week02a, p.17, p.27).
- Complete: a reader can tell what the actor is trying to do and how it ends.
- Consistent: it does not contradict another use case, and any branch it names
  lines up with a scenario in the related user story (Week02a, p.17).
- Verifiable: you can imagine a test that confirms the goal was reached
  (Week02a, p.17, p.20).
- Traceable: it has an ID and lists the user stories it leads to (Week02a, p.21).

Also avoid the lecture's pitfalls: do not confuse a requirement with a solution,
and do not add unrequested goals, which the lecture calls gold plating
(Week02a, p.27).

### Optional but recommended: a use case diagram

A high-level use case list pairs well with a use case diagram, which is the
classic one-page view of the system. The requirements ask for UML diagrams in
your documents (requirements.md:7), and the lecture uses UML notation for the
domain model (Week02a, p.14, p.16), so a UML use case diagram fits the course.

You do not draw it in this file. Build it in a diagram tool and describe it as:

- Stick figures on the outside for each actor (Member, Poster, Claimant, Admin,
  Guest).
- An oval inside the system boundary for each use case (UC-01, UC-02, ...).
- A line connecting an actor to each use case that actor takes part in.

This gives reviewers a fast map of who does what, which supports the role-
coverage check they will run (requirements.md:341).

## Part 4: Worked example

This example uses the project domain (the Local Produce Exchange) and follows
the template above. The branches named here map directly to Sample User Story #1
in the requirements (requirements.md:43-67), which keeps the two artifacts
consistent.

### Use Case UC-04: Claim produce from a listing

- Primary actor: Claimant
- Supporting actors: Poster (owns the listing)
- Goal: Request a specific quantity from an active listing so the claimant
  receives only the amount they need.
- Preconditions: The claimant is a registered, active member. The listing is
  active and has a quantity available.
- Trigger: The claimant opens an active listing and chooses to request it.
- Main success flow:
  1. The claimant opens an active listing.
  2. The claimant enters the quantity they want.
  3. The system checks the amount against the quantity that remains.
  4. The system creates a claim with status REQUESTED.
  5. The system notifies the poster of the new request.
- Alternate and exception flows:
  - Requested amount is more than what remains: the system rejects the request
    and creates no claim.
  - Requested amount is zero or negative: the system rejects it and shows a
    validation error.
  - The member is suspended: the system denies access and creates no claim.
- Postconditions: A claim exists with status REQUESTED for the requested amount,
  and the poster has been notified.
- Related user stories: US-12 (submit a claim for a specified quantity).

## Part 5: Checklist before you submit

For each use case:

- [ ] Has a UC-<number> id and a verb-phrase name.
- [ ] Primary actor is a role the project defines.
- [ ] States a one-sentence goal (the "what," not the "how").
- [ ] Lists preconditions and a trigger.
- [ ] Has a short, numbered main success flow.
- [ ] Names the important alternate and exception flows as brief bullets.
- [ ] States postconditions.
- [ ] Lists the related user stories (or marks them TBD).
- [ ] Stays high level: no long branch detail that belongs in a user story.
- [ ] Is unambiguous and verifiable (no vague words like "fast").
- [ ] Describes system behavior, not a specific UI solution.

For the full set of use cases:

- [ ] Every defined role (member, poster, claimant, admin, guest) appears as the
      primary actor of at least one use case.
- [ ] No two use cases contradict each other.
- [ ] A use case diagram exists showing actors and their use cases.

## Part 6: Master use case list (optional summary table)

A short table gives reviewers the whole picture at a glance. Fill one row per
use case.

| Use case ID | Name (verb phrase) | Primary actor | Goal (one line) |
| --- | --- | --- | --- |
| UC-01 | <name> | <role> | <goal> |
| UC-02 | <name> | <role> | <goal> |
| UC-04 | Claim produce from a listing | Claimant | Request a set quantity from an active listing |

## Sources

Project requirements document (ics613-research/requirements/requirements.md):

- High-level use cases as a Core Requirement (line 148); full Core Requirements
  list (lines 147-154).
- UML diagrams expected in project documents (line 7).
- Project roles: member, poster/claimant, admin (lines 33-35); roles to cover in
  review (line 341).
- Sample User Story #1, whose scenarios the worked example maps to (lines 43-67).
- Use cases named in the grading focus (line 218) and presentation content
  (line 254).

Week 02 Requirements Engineering lecture
(ics613-research/lectures/Week02a_RequirementsEngineering.pdf):

- Use cases listed as a specification artifact (p.2).
- Stakeholder and role identification (p.9).
- Acceptance criteria: edge, error, and permission paths belong here (p.12).
- Domain modeling; actors and behavior derived from nouns and verbs (p.14-15).
- Writing good requirements: Correct, Unambiguous, Complete, Consistent,
  Verifiable, Traceable (p.17).
- The system description is what, not how (p.18).
- Validation: if you cannot write a test for it, it is likely flawed (p.20).
- Traceability from requirement to design to test (p.21).
- Common pitfalls: vague requirements, confusing requirements with solutions,
  gold plating (p.27).
- Further Reading, the source of the standard use case format: UML Distilled and
  the SWEBOK Requirements Knowledge Area (p.28).
