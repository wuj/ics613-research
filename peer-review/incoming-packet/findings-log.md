# Findings Log

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu\
**Reviewed team:** Team Green Beans

Internal log of every finding from the review of Team Green Beans' packet (incoming-packet/Cross-Team Requirements Review Packet.pdf, 11 pages). Findings group by category with severity descending. Page numbers refer to that PDF; assignment citations refer to ics613-research/requirements.md; lecture citations refer to ics613-research/lectures/Week02a_RequirementsEngineering.pdf.

### F-01: Uniform five-part acceptance criteria on every story
- Category: Strength
- Severity: n/a
- Location: Cross-Team Requirements Review Packet.pdf, User Stories and Acceptance Criteria, stories 1-28, pages 1-8; example story 26 item d, page 7
- Quote: "If the quantity entered is negative or invalid, the system displays a validation error. If the quantity is changed to zero, the system prompts the owner to mark the listing as unavailable or closed." (story 26 item d, page 7)
- Finding: Every story carries the same five labeled parts: Specific Behavior, Observable Result, Normal Path, Edge/Error Paths, and Permission Rules. This matches the acceptance criteria checklist in the course lecture exactly (Week02a_RequirementsEngineering.pdf, page 12, "What to include": specific behavior, observable result, normal path, edge/error paths, permission rules), and the parts are filled with concrete, testable rules.
- Question: none

### F-02: Open questions target the packet's largest gaps
- Category: Strength
- Severity: n/a
- Location: Cross-Team Requirements Review Packet.pdf, Open Questions, pages 9-10
- Quote: "What is the exact listing status lifecycle? Our user stories mention statuses like available, reserved, unavailable, closed, canceled, completed, accepted, denied, and removed." (Open Questions, page 9)
- Finding: The four open questions (status lifecycle, role model, inventory conflicts, image storage) point at the same issues this review rates highest (F-07, F-19, F-23, F-28). The team has located its own gaps; the questions lack answers, owners, or deadlines, which the related findings cover.
- Question: none

### F-03: Measurable performance assumption
- Category: Strength
- Severity: n/a
- Location: Cross-Team Requirements Review Packet.pdf, Known Assumptions, page 10
- Quote: "Site navigation should respond in 3 seconds or less, and search should return results in under 1 second for 1,000 food listings." (Known Assumptions, page 10)
- Finding: Concrete numbers make this assumption directly testable, the verifiable quality the course lecture asks of a requirement (Week02a_RequirementsEngineering.pdf, page 17, "Verifiable: can be tested or demonstrated to confirm it is met").
- Question: none

### F-04: Personas are concrete and map to story groups
- Category: Strength
- Severity: n/a
- Location: Cross-Team Requirements Review Packet.pdf, Personas, personas 1-4, pages 8-9; example persona 2, pages 8-9
- Quote: "Creates food listings for excess produce, uploads photos, enters quantity and freshness information, updates remaining quantity, marks listings reserved or unavailable, joins gardening and sustainability communities, and communicates with claimants through the messaging system." (Personas, persona 2, pages 8-9)
- Finding: All four personas carry demographics, goals, technology habits, pain points, and use cases, and each use-case list maps to a concrete story group (Lily to stories 18, 19, 25, 26; Oliver to stories 13, 14, 20, 21; Rose to stories 3, 7, 9, 22, 28; Glen to stories 6, 20).
- Question: none

### F-05: Domain model included with named relationships and multiplicities
- Category: Strength
- Severity: n/a
- Location: Cross-Team Requirements Review Packet.pdf, Domain Model, page 11
- Quote: "User, Community, Membership, Listing, ClaimRequest, MessageThread, Message, Notification, Review, Photo, Invitation" (Domain Model entity labels, page 11)
- Finding: The optional domain model is included; the assignment calls it "a nice-to-include item" (requirements.md, Artifacts for review section). Its entities match the stories, its relationships are named, and multiplicities appear on the relationship ends, which is what the course lecture asks of a domain model (Week02a_RequirementsEngineering.pdf, page 15, "every relationship should be named and have multiplicity (cardinality)").
- Question: none

### F-06: Two conflicting exchange models
- Category: Ambiguity
- Severity: High
- Location: Cross-Team Requirements Review Packet.pdf, story 12 item a, page 4; Known Assumptions, page 10; stories 24-26, page 7; Domain Model, page 11
- Quote: "The system automatically tracks available quantity and prevents approvals that would exceed the remaining inventory." (story 12 item a, page 4) "Claims: Exchange coordination is expected to happen through DMs, with listing owners able to mark a listing as reserved." (Known Assumptions, page 10)
- Finding: Stories 12 and 14 plus the ClaimRequest entity (status, quantityRequested attributes, page 11) describe a system-managed claim flow, while the Claims assumption and stories 24-26 describe manual coordination by direct message with owner-edited quantities. The assignment requires the system-managed behavior (requirements.md, Project: Local Produce Exchange section: "The system must prevent conflicting claims by ensuring that approved quantities never exceed the remaining quantity."), so the manual model alone would not meet it.
- Question: Which exchange model is being built, and if both exist, when does each apply?

### F-07: Status lifecycle undefined and departs from the assignment's sequence
- Category: Ambiguity
- Severity: High
- Location: Cross-Team Requirements Review Packet.pdf, Open Questions, page 9; story 8 item b, page 3; story 14 item a, page 4
- Quote: "What is the exact listing status lifecycle? Our user stories mention statuses like available, reserved, unavailable, closed, canceled, completed, accepted, denied, and removed." (Open Questions, page 9) "A list of all previous and current exchanges that display the produce, date exchanged, quantity, and status (accepted, cancelled, completed, etc.)." (story 8 item b, page 3)
- Finding: Nine status names appear with no transition rules, story 8's "etc." leaves the set open, and the spelling drifts between "canceled" (page 9) and "cancelled" (page 3). The assignment already defines the request sequence REQUESTED -> APPROVED -> PICKED_UP -> COMPLETED, with REQUESTED -> DENIED terminal and CANCELLED allowed only from REQUESTED or APPROVED (requirements.md, Project: Local Produce Exchange section); no packet story uses a picked-up status, and story 14 (page 4) allows cancellation until completion, contradicting the assignment's cancellation-before-pickup rule.
- Question: What is the agreed status list, and which transitions are legal?

### F-08: Open registration and public communities versus the invite-only requirement
- Category: Ambiguity
- Severity: High
- Location: Cross-Team Requirements Review Packet.pdf, story 15 item e, page 5; story 1 item e, page 1; stories 22-23, pages 6-7
- Quote: "Any visitor can register for an account. Only registered users can access authenticated features." (story 15 item e, page 5) "Any logged-in user can request to join a public community." (story 1 item e, page 1)
- Finding: The assignment defines the product as "an invite-only exchange of surplus produce and other food items within a trusted private community" and says "Users join only via invitation" (requirements.md, Project: Local Produce Exchange section); its sample test case TC-002 registers with an invite token. The packet provides invitations only for private communities (stories 22-23) while registration itself and public communities stay open.
- Question: Are open registration and public communities intentional departures from the invite-only requirement?

### F-09: Story 25's normal path describes a different feature than its behavior
- Category: Ambiguity
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 25 items a and c, page 7
- Quote: "Listing owner can change a listing status from "Available" to "Reserved."" (story 25 item a, page 7) "Owner opens dashboard -> views incoming requests -> selects approve or deny -> system updates request status -> claimant receives notification." (story 25 item c, page 7)
- Finding: The behavior and result lines specify marking a listing reserved; the normal path describes approving or denying claim requests. The two halves do not describe the same feature, which fails the consistency quality the course lecture names for requirements (Week02a_RequirementsEngineering.pdf, page 17, "Consistent: does not contradict any other requirement").
- Question: Which flow is story 25 specifying, marking a listing reserved or approving requests?

### F-10: Story 22 contradicts itself on who can invite
- Category: Ambiguity
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 22 items a and e, page 6
- Quote: "A community member or admin can send an invitation by entering the recipient's email address." (story 22 item a, page 6) "Only community hosts or admins can generate invite links." (story 22 item e, page 6)
- Finding: The behavior line lets members send invitations while the permission line restricts invite links to hosts or admins. "Host" appears here and in the Rose persona but is never defined relative to "admin"; the packet's own role-model open question (page 9) lists both without resolving them.
- Question: Can ordinary members send invites, and is "host" a distinct role from "admin"?

### F-11: Kick versus ban undefined, and ban scope unclear
- Category: Ambiguity
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 9 item a, page 3; story 7 item e, page 3; story 1 item e, page 1; story 5 item d, page 2
- Quote: "Admins can select a user's listing with a "remove" option or a user's account with a "kick" or "ban" option." (story 9 item a, page 3) "Banned users cannot post or view the community page." (story 7 item e, page 3)
- Finding: Story 9 offers kick and ban as separate options without defining the difference. Story 1 (page 1) says banned users cannot rejoin "the same community", implying per-community bans, while story 5 (page 2) disables message threads when an account "has been removed or banned", implying an account-level state.
- Question: Is a ban per community or platform-wide, and what does it add beyond a kick?

### F-12: Guest access undefined behind "All users"
- Category: Ambiguity
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 20 item e, page 6; story 13 item e, page 4
- Quote: "All users can browse and search listings." (story 20 item e, page 6) "All users may view expiration information for public listings." (story 13 item e, page 4)
- Finding: The assignment's review guidance asks whether member, owner/poster, admin, and guest are all accounted for (requirements.md, Conducting the review section). The packet defines no guest role, and "All users" never says whether signed-out visitors can browse; public visibility of pickup details is a privacy question the packet does not address.
- Question: Can a person who is not signed in browse listings and see pickup details?

### F-13: Stories 4, 21, and 24 overlap on one messaging feature
- Category: Ambiguity
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 4, page 1; story 21, page 6; story 24, page 7; stories 5-6, page 2
- Quote: "As a user claiming a food item, I want to message the listing owner, so that I can coordinate pickup details." (story 4, page 1) "As a user interested in a listing, I want to privately message the listing owner, so that I can arrange pickup details and request a specific quantity of the item." (story 21, page 6) "As a user interested in a listing, I want to message the listing owner with the quantity I am interested in, so that we can coordinate the exchange manually." (story 24, page 7)
- Finding: Three stories cover messaging a listing owner from a listing with differing details (story 4 limits it to claimants, story 21 to any interested user, story 24 adds a structured quantity). The instructor's clarification says the 25-30 stories should represent distinct new functionalities, not splits of one feature (requirements.md, Clarifications section, answer A1). Stories 5 and 6 (page 2) overlap similarly on receiving and listing messages.
- Question: Which messaging story is authoritative, and are the others variants to fold into its acceptance criteria?

### F-14: Notification retry unbounded and delivery channel undecided
- Category: Ambiguity
- Severity: Low
- Location: Cross-Team Requirements Review Packet.pdf, story 11 items b and d, page 4
- Quote: "If notification delivery fails, notification remains queued and is retried." (story 11 item d, page 4) "A notification appears in the user's notification center and/or is sent through email." (story 11 item b, page 4)
- Finding: The retry has no limit, interval, or terminal failure state, and "and/or" leaves the channel decision open per event type.
- Question: How many retries happen over what period, and which events go to email versus the notification center?

### F-15: Rating aggregation undefined
- Category: Ambiguity
- Severity: Low
- Location: Cross-Team Requirements Review Packet.pdf, story 10 item b, page 3
- Quote: "The review appears on the recipient's profile, and their rating is updated." (story 10 item b, page 3)
- Finding: The story does not say how the displayed rating is computed from individual reviews (average, count, recency weighting), which a builder needs to decide done.
- Question: How is the displayed rating computed from individual reviews?

### F-16: Dietary and allergen tags absent though the assignment requires them
- Category: Missing case
- Severity: High
- Location: Cross-Team Requirements Review Packet.pdf, story 18 item a, page 5; coverage across stories 1-28 (pages 1-8), Known Assumptions (page 10), and Domain Model (page 11)
- Quote: "User can create a listing including produce name, category, quantity, expiration date, pickup information, and photos." (story 18 item a, page 5)
- Finding: The assignment requires listings to carry "dietary/allergen tags (e.g., vegan, contains nuts, gluten-free)" among the required listing details (requirements.md, Project: Local Produce Exchange section). No story, assumption, or domain model attribute mentions dietary or allergen information anywhere in the packet. For a food exchange this is also a safety gap.
- Question: Where is allergen and dietary information captured, and is it required when posting a listing?

### F-17: No story creates a community
- Category: Missing case
- Severity: High
- Location: Cross-Team Requirements Review Packet.pdf, Personas, persona 3, page 9; coverage across stories 1-28, pages 1-8
- Quote: "Creates or hosts a community, posts announcements on the community discussion board, invites trusted members, approves or rejects private community requests, manages member roles, and removes inappropriate posts or bad-faith users." (Personas, persona 3, page 9)
- Finding: Stories 1, 2, 3, 9, 22, and 28 join, manage, or moderate communities that already exist, and the Rose persona's first use case is creating one, but no story among 1-28 creates a community or assigns its first admin. This is a backlog dependency gap of the kind the assignment's review guidance asks reviewers to find (requirements.md, Conducting the review section: "Does any story depend on elements not included in the backlog?").
- Question: Who can create a community, and what does creation set up (first admin, privacy setting, invite rules)?

### F-18: No story marks an exchange picked up or completed
- Category: Missing case
- Severity: High
- Location: Cross-Team Requirements Review Packet.pdf, story 10 item c, page 3; story 14 item a, page 4; coverage across stories 1-28, pages 1-8
- Quote: "Exchange is marked as completed" (story 10 item c, page 3) "Users can cancel pending or approved requests before the exchange is marked as completed." (story 14 item a, page 4)
- Finding: Stories 10 and 14 both depend on a completed state, and the assignment expects dashboard actions for "approve/deny, cancel, picked up, completed" (requirements.md, Project: Local Produce Exchange section), but no story provides the act of marking an exchange picked up or completed or names who performs it.
- Question: Which user marks an exchange picked up and completed, and through which screen?

### F-19: Moderator role exists with no defined abilities
- Category: Missing case
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 28 item a, page 8; coverage across stories 1-28, pages 1-8
- Quote: "Community admin can assign or remove roles such as member, moderator, or admin." (story 28 item a, page 8)
- Finding: Story 28 assigns the moderator role and the role-model open question (page 9) lists it, but no story grants moderators any capability; "moderator" appears only in story 28 and the open questions.
- Question: What can a moderator do that a member cannot?

### F-20: The assignment's admin capabilities have no stories
- Category: Missing case
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 5 item d, page 2; stories 9 and 28, pages 3 and 8; coverage across stories 1-28, pages 1-8
- Quote: "If the claimant's account has been removed or banned, the message thread is disabled." (story 5 item d, page 2)
- Finding: The assignment gives admins three abilities: "suspend users, deactivate listings (hide from browsing without deleting audit history), and generate basic reports" (requirements.md, Project: Local Produce Exchange section). The packet's admin stories (9, 28) cover per-community removal and role changes only; no story suspends an account, deactivates a listing while preserving history (story 19 deletes outright), or produces reports, while story 5's edge path already depends on an account-level removal nothing provides.
- Question: Who suspends accounts and generates reports, and what happens to a suspended user's active listings and requests?

### F-21: Persona search needs exceed story 20
- Category: Missing case
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, Personas, persona 1 item g, page 8; persona 4 item g, page 9; story 20 item a, page 6
- Quote: "Searches and filters nearby food listings by category, pickup area, quantity, and freshness" (Personas, persona 1 item g, page 8) "Searches for available local donations by category, quantity, expiration date, and pickup window" (Personas, persona 4 item g, page 9) "User can enter keywords or select categories to filter food listings." (story 20 item a, page 6)
- Finding: Both food-seeking personas filter by attributes (pickup area, freshness, quantity, expiration date, pickup window) that story 20's keyword-and-category search does not provide. The assignment also names "an available pickup window" as a required listing detail (requirements.md, Project: Local Produce Exchange section), which story 18's looser "pickup information" may or may not cover.
- Question: Which filters does search support at launch, and is the pickup window a structured field?

### F-22: Permission rules lack enforcement scenarios
- Category: Missing case
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, coverage across stories 1-28, pages 1-8; lone exception story 28 item d, page 8
- Quote: none (coverage: every story states Permission Rules, but across stories 1-28 the only Edge/Error Path that enforces one is story 28's block on an admin removing their own only-admin role, page 8; no story specifies the system's response to an unauthorized attempt, such as story 8's rule that users cannot view other users' listing history)
- Finding: The instructor's clarification says permissions should be captured as acceptance criteria scenarios, for example a non-owner being denied (requirements.md, Clarifications section, answer A1). The packet states who may act but not the observable denial behavior, so the rules cannot be tested as written.
- Question: What does a user see when they attempt an action their role does not allow?

### F-23: Double-claim race unresolved in the manual flow
- Category: Missing case
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, Open Questions, page 10; story 24 item d, page 7; story 26, page 7
- Quote: "How will inventory conflicts be prevented?" (Open Questions, page 10) "If the quantity entered is greater than the listed available quantity, the system displays a warning before sending." (story 24 item d, page 7)
- Finding: The only guard fires when a message is sent, against a quantity the owner edits by hand (story 26), so two claimants can arrange the same units at the same time. The assignment requires the system to prevent conflicting claims (requirements.md, Project: Local Produce Exchange section). The open question is asked but no story answers it.
- Question: What reserves quantity between an agreement in messages and the pickup?

### F-24: Known risks cover process only
- Category: Missing case
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, Known Risks, page 10
- Quote: none (coverage: the Known Risks list, page 10, holds five entries: falling behind schedule, scope creep, code conflicts, late-stage bugs, and permissions and security; every entry names a team-process exposure, and none names a technical or product exposure such as image storage, email delivery, or the messaging build, although the Open Questions on page 10 raise image storage and inventory conflicts)
- Finding: The risk list tracks how the team works, while the packet's own open questions point at technical exposures that never appear as risks with mitigations.
- Question: Which technical builds does the team consider riskiest, and what is the fallback for each?

### F-25: Discussion-board posts have no entity in the domain model
- Category: Missing case
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 7 item b, page 2; Domain Model, page 11
- Quote: "The post appears on the community discussion board and is visible to the members of that community." (story 7 item b, page 2)
- Finding: Story 7 publishes posts to a community board, but the domain model contains Message and MessageThread for private conversations and no entity for a board post or announcement, so a concept the stories rely on is absent from the model the lecture says should capture the domain's key entities (Week02a_RequirementsEngineering.pdf, pages 14-15).
- Question: Which entity stores discussion-board posts and their moderation state?

### F-26: Story 23 lacks a normal path
- Category: Missing case
- Severity: Low
- Location: Cross-Team Requirements Review Packet.pdf, story 23, pages 6-7
- Quote: none (coverage: story 23 lists Specific Behavior, Observable Result, Edge/Error Paths, and Permission Rules; it is the only story in the packet without a Normal Path item)
- Finding: The missing path leaves the invited user's normal sequence unspecified: open the link, signed in or not, confirm, join.
- Question: What are the normal-path steps for accepting an invitation, including for a person who has no account yet?

### F-27: No sign-in or sign-out story
- Category: Missing case
- Severity: Low
- Location: Cross-Team Requirements Review Packet.pdf, stories 15-16, page 5; coverage across stories 1-28, pages 1-8
- Quote: none (coverage: story 15, page 5, registers and immediately logs in a new user; story 16, page 5, ends with the user able to "log in with new credentials"; no story among 1-28 covers signing in to an existing account or signing out)
- Finding: Both account stories assume a login feature no story provides, the backlog dependency gap the assignment's review guidance asks reviewers to check (requirements.md, Conducting the review section: "Does any story depend on elements not included in the backlog?").
- Question: Is sign-in and sign-out treated as part of story 15, or does it need its own story?

### F-28: Hosting and storage plan undecided while stories depend on it
- Category: Scope risk
- Severity: High
- Location: Cross-Team Requirements Review Packet.pdf, Open Questions, page 10; Known Assumptions, page 10; stories 17-18, page 5
- Quote: "Where will images be stored?" (Open Questions, page 10) "Deployment: Vercel" (Known Assumptions, page 10)
- Finding: Stories 17 and 18 (page 5) require image upload, the deployment assumption names a platform with no storage or backend plan, and the assignment mandates a Python backend (FastAPI, SQLAlchemy, Pydantic) with PostgreSQL (requirements.md, Overview section). Image storage, database hosting, and where the Python backend runs are undecided or unstated, and listing creation cannot be built end to end until they are.
- Question: Where will images, the database, and the backend service run?

### F-29: The messaging subsystem is a large build
- Category: Scope risk
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, stories 4-6, pages 1-2; story 21, page 6; story 24, page 7
- Quote: "The inbox displays conversation participants, related listing title, most recent message preview, time stamp, and unread message status." (story 6 item b, page 2)
- Finding: Five stories specify threads, an inbox with previews and unread status, listing details pinned to threads, threads disabled after bans, and structured quantity messages. That is a sizable subsystem for a course timeline, a feature the assignment's review guidance flags as a scope risk (requirements.md, Review feedback section: "features that seem too large or technically risky").
- Question: Which messaging features are required for the first release, and which can wait?

### F-30: Outbound email is an external dependency with no plan
- Category: Scope risk
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, story 11 item b, page 4; story 16, page 5; story 22, page 6; Known Assumptions, page 10
- Quote: "A notification appears in the user's notification center and/or is sent through email." (story 11 item b, page 4)
- Finding: Notifications (story 11), password reset (story 16), and invitations (story 22) all send email, but no assumption names a sending service and the only failure handling is story 11's unbounded retry.
- Question: Which service sends email, and what happens when delivery fails permanently?

### F-31: No priority order across the 28 stories
- Category: Scope risk
- Severity: Medium
- Location: Cross-Team Requirements Review Packet.pdf, Known Risks, page 10; coverage across stories 1-28, pages 1-8
- Quote: "Scope creep: Prioritize core functionality and postpone non-essential features if timeline constraints arise." (Known Risks, page 10)
- Finding: The mitigation depends on knowing what is core, but no story carries a priority or release marker, and with two exchange models in the packet (F-06) the smallest buildable set is undefined.
- Question: Which stories form the smallest first release?
