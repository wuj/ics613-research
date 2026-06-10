# Requirements Review Presentation Outline

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu\
**Reviewed team:** Team Green Beans

Total time: 10 minutes.

## Slide 1: Title and reviewed team
- Introduce Team 4 (Local Produce Exchange, "Surplus") and the review target: Team Green Beans' requirements packet.
- One line on the assignment: each team reviews the other team's requirements before implementation begins, to catch problems while they are still cheap to fix.
- Time: 0:30

## Slide 2: What we reviewed and how
- The packet: one 11-page document with 28 user stories and acceptance criteria, 4 personas, open questions, known assumptions, known risks, and a domain model. Every required item is present.
- Method: the assignment's five review lenses (stories, acceptance criteria, roles, edge cases, consistency) plus two more for the open questions and the risks, with the Week 02a lecture checklists as the yardstick.
- Grounding rule: every finding cites the packet page it comes from, and findings that rest on the assignment or the lecture cite that source too.
- Time: 1:00

## Slide 3: Summary impression
- A disciplined packet: uniform five-part acceptance criteria on all 28 stories, concrete personas, and the optional domain model included.
- Main theme: two exchange models live side by side, system-tracked claim requests versus manual coordination by direct message.
- Several decisions are undefined or depart from the course project description: invite-only access, the request status sequence, and required listing fields.
- Time: 1:00

## Slide 4: Strengths (F-01 to F-05)
- F-01: every story uses the five-part acceptance criteria checklist straight from the Week 02a lecture, page 12.
- F-02: the team's own open questions name the deepest gaps; our review confirms the same four spots.
- F-03: measurable performance targets (3-second navigation, 1-second search at 1,000 listings), the lecture's verifiable quality, page 17.
- F-04: personas with goals, devices, and task lists that map onto story groups.
- F-05: domain model with named relationships and multiplicities, matching the lecture's domain model rules, page 15.
- Time: 1:30

## Slide 5: Ambiguities (F-06 to F-13)
- F-06 (high): story 12 has the system enforce claim quantities; the Claims assumption says coordination happens in DMs. The assignment requires system-enforced claim prevention, so the choice decides compliance.
- F-07 (high): nine status names, no transition rules, and no picked-up status; the assignment fixes REQUESTED -> APPROVED -> PICKED_UP -> COMPLETED and forbids cancellation after pickup.
- F-08 (high): open registration (story 15) and public communities (story 1) versus the assignment's invite-only requirement.
- Mediums in one pass: story 25's mismatched normal path (F-09), member-versus-host invites (F-10), kick versus ban (F-11), guest access behind "All users" (F-12), three overlapping messaging stories (F-13).
- Time: 2:00

## Slide 6: Missing cases (F-16 to F-23)
- F-16 (high): no dietary or allergen information anywhere; the assignment requires allergen tags on listings; safety angle for a food app.
- F-17 (high): no story creates a community; six stories and the Rose persona assume one exists.
- F-18 (high): nothing marks an exchange picked up or completed; reviews (story 10) and the cancellation window (story 14) both hang on that mark.
- Mediums in one pass: moderator role with no powers (F-19), missing admin suspension and reports (F-20), persona search filters story 20 lacks (F-21), no permission-denial scenarios (F-22), double-claim race in the manual flow (F-23).
- Time: 2:00

## Slide 7: Scope risks (F-28)
- F-28 (high): image storage is an open question while stories 17-18 require uploads, and the Vercel assumption has no plan for the required Python FastAPI backend and PostgreSQL database. Listing creation is blocked end to end until hosting is decided.
- From the log, lower-severity risks to mention briefly: messaging subsystem size (F-29), outbound email dependency (F-30), no priority order across 28 stories (F-31).
- Time: 1:00

## Slide 8: Questions for the team
- Which exchange model is being built, and if both exist, when does each apply? (F-06)
- What is the agreed status list, and which transitions are legal? (F-07)
- Should every entry point require an invitation? (F-08)
- Where is allergen and dietary information captured? (F-16)
- Which user marks an exchange picked up and completed? (F-18)
- Where will images, the database, and the backend service run? (F-28)
- Time: 0:45

## Slide 9: Closing
- A strong, uniform packet; the fixes are decisions rather than rewrites: pick one exchange model, adopt the assignment's status sequence, and close the invite-only gap.
- Time: 0:15
