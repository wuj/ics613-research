# Requirements Review Report

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu\
**Reviewed team:** Team Green Beans

## Summary

We reviewed the requirements packet from Team Green Beans, an eleven-page document holding 28 numbered user stories with acceptance criteria, four personas, open questions, known assumptions, known risks, and a domain model diagram. A user story is a one-sentence description of a task a user wants to do. Acceptance criteria are the conditions that tell the team when that story is done. A persona is an example user who stands in for a real audience, and a domain model is a diagram of the main concepts the app handles and how they connect. The packet covers every item the review assignment asks for.

The packet's strongest property is its uniform structure: every story carries the same five labeled parts, so the stories are comparable and testable. The findings cluster around three themes. First, the packet describes two conflicting ways an exchange happens: a system-tracked claim request with automatic quantity checks, and a manual path where people coordinate by private message. Second, core decisions are undefined or depart from the course project description, including the status lifecycle and invite-only access. Third, several required features appear in no story, and the image storage and hosting decisions are still open.

## Strengths

**F-01:** Every story uses the same five-part acceptance criteria layout: specific behavior, observable result, normal path, edge or error paths, and permission rules. This matches the checklist in the Week 2 lecture on requirements engineering (page 12), and the parts hold concrete rules, for example story 26: "If the quantity entered is negative or invalid, the system displays a validation error." (page 7).

**F-02:** The open questions name the packet's deepest gaps, for example: "What is the exact listing status lifecycle?" (page 9). The four questions match the largest issues this review found.

**F-03:** The performance assumption is measurable: "Site navigation should respond in 3 seconds or less, and search should return results in under 1 second for 1,000 food listings." (page 10). Concrete numbers are directly testable, the verifiable quality the Week 2 lecture asks of a requirement (page 17).

**F-04:** The personas are specific people whose task lists map onto story groups. Lily, the home gardener, "Creates food listings for excess produce, uploads photos, enters quantity and freshness information, updates remaining quantity, marks listings reserved or unavailable" and messages claimants (pages 8-9).

**F-05:** The packet includes a domain model diagram (page 11) though the assignment lists it as optional. Its eleven entities match the stories, and its relationships are named and carry counts on both ends, which is what the Week 2 lecture asks of a domain model (page 15).

## Ambiguities

An ambiguity is a requirement that can be read in more than one way. The two largest decide how the exchange flow works.

**F-06, high:** The packet describes two conflicting exchange models. Story 12 is system-managed: "The system automatically tracks available quantity and prevents approvals that would exceed the remaining inventory." (page 4). The known assumptions say the opposite: "Claims: Exchange coordination is expected to happen through DMs, with listing owners able to mark a listing as reserved." (page 10), and stories 24 to 26 build that manual path. The course project description requires the system itself to prevent conflicting claims, so this choice decides whether that requirement is met. Which model is being built, and if both exist, when does each apply?

**F-07, high:** The status lifecycle is undefined and departs from the assignment. The team's own question lists nine status names (page 9), story 8 leaves the list open with "status (accepted, cancelled, completed, etc.)" (page 3), and the spelling drifts between "canceled" and "cancelled". The course project description fixes the sequence REQUESTED -> APPROVED -> PICKED_UP -> COMPLETED, with cancellation allowed only before pickup. No story uses a picked-up status, and story 14 allows cancelling any time before completion (page 4), which contradicts that rule. What is the agreed status list, and which transitions are legal?

**F-08, high:** Account and community access conflict with the assignment's access rule. The course project description requires an invite-only exchange inside a trusted private community, where "Users join only via invitation". The packet allows open sign-up, "Any visitor can register for an account." (story 15, page 5), and public communities, "Any logged-in user can request to join a public community." (story 1, page 1). Are these intentional departures, or should every entry point require an invitation?

**F-09, medium:** Story 25's behavior line has the owner mark a listing reserved, but its normal path describes a different flow: "Owner opens dashboard -> views incoming requests -> selects approve or deny" (both page 7). Which flow is story 25 specifying?

**F-10, medium:** Story 22 contradicts itself about who can invite: "A community member or admin can send an invitation" versus "Only community hosts or admins can generate invite links." (both page 6). Can ordinary members invite, and is a host a different role from an admin?

**F-11, medium:** The difference between a kick and a ban is undefined. Story 9 offers both options for an account (page 3), and story 7 says "Banned users cannot post or view the community page." (page 3). Is a ban limited to one community or the whole platform, and what does it add beyond a kick?

**F-12, medium:** "All users" leaves guest access undefined. Story 20 says "All users can browse and search listings." (page 6) and story 13 extends viewing to public listings (page 4). The assignment's review guidance asks whether the guest role is covered; the packet never says whether a signed-out visitor can browse, which also decides whether pickup details are publicly visible. Can signed-out visitors browse listings?

**F-13, medium:** Stories 4, 21, and 24 are three versions of one feature, messaging a listing owner from a listing, with different rules (pages 1, 6, 7). The instructor's clarification on story counts says the 25 to 30 stories should represent distinct new functionalities. Which version is authoritative?

## Missing Cases

A missing case is a needed behavior, rule, or failure path that no story covers. The first three involve features the course project description requires.

**F-16, high:** Nothing in the packet mentions dietary or allergen information. Story 18 lists the listing fields: "produce name, category, quantity, expiration date, pickup information, and photos" (page 5). The course project description requires dietary and allergen tags on listings, with examples like "vegan, contains nuts, gluten-free". For a food exchange this is also a safety gap. Where does allergen information live, and is it required at posting time?

**F-17, high:** No story creates a community. Stories 1, 2, 3, 9, 22, and 28 join, manage, or moderate communities that already exist, and the Rose persona expects to create one: "Creates or hosts a community" (page 9). Without a creation story there is no path to the first community. Who can create a community, and what does creation set up, including its first admin?

**F-18, high:** No story marks an exchange picked up or completed, yet two stories depend on that mark: story 10's flow starts with "Exchange is marked as completed" (page 3), and story 14 limits cancellation to "before the exchange is marked as completed" (page 4). The course project description expects dashboard actions for approve or deny, cancel, picked up, and completed. Who marks these states, and when?

**F-19, medium:** Story 28 lets admins assign a moderator role (page 8), but no story gives moderators any ability, so the role has no defined powers. What can a moderator do that a member cannot?

**F-20, medium:** The admin abilities in the course project description, suspending users, deactivating listings while keeping their history, and producing basic reports, have no stories. Story 5 depends on an account-level removal that nothing provides: "If the claimant's account has been removed or banned, the message thread is disabled." (page 2). Who suspends accounts, and what happens to their active listings and requests?

**F-21, medium:** The personas search in ways no story supports. Oliver filters by "category, pickup area, quantity, and freshness" (page 8) and Glen by "category, quantity, expiration date, and pickup window" (page 9), but story 20 offers keywords and categories only (page 6). Are location, freshness, and pickup-window filters in scope?

**F-22, medium:** Permission rules lack enforcement scenarios. Across all 28 stories, the only error path enforcing a permission rule is story 28's block on removing the last admin (page 8). The instructor's clarification asks for permissions as acceptance criteria scenarios, such as a non-owner being denied. What should a user see when they attempt an action their role does not allow?

**F-23, medium:** In the manual flow, nothing stops two people from arranging the same produce at the same time. The open question asks "How will inventory conflicts be prevented?" (page 10), and the only guard is a warning when the message is sent (page 7), not a hold on quantity. The course project description requires the system to prevent conflicting claims. What reserves quantity between an agreement in messages and the pickup?

## Scope Risks

A scope risk is a feature that is larger or riskier to build than the packet treats it.

**F-28, high:** The hosting and storage plan is undecided while several stories depend on it. The open question "Where will images be stored?" (page 10) is unanswered, yet story 17 includes profile pictures and story 18 requires photo upload. The assumptions name "Deployment: Vercel" and "Database: Postgres" (page 10), but the course requires a Python backend using FastAPI with PostgreSQL, and that combination plus image files needs a hosting answer before listing creation can be built end to end. Where do images, the database, and the backend run?
