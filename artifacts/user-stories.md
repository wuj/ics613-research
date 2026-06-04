# User Stories: Local Produce Exchange

## 1. Introduction

This document is the "25 to 30 user stories (with acceptance criteria)"
artifact. It is one of the Core Requirements for the inception deliverable due
June 8 (requirements.md:148-153). Each story names a user, the goal that user
wants, and the benefit they get, followed by acceptance criteria written as
Gherkin scenarios. The format follows our team template
(user-story-template.md:24-26) and the Week 02 Requirements Engineering lecture,
"User stories" slide, p.11.

The stories below are drawn from three aligned sources: the high-level use cases
(use-cases.md), the project requirements including the claim lifecycle and
business rules (requirements.md:33-35), and the team charter In Scope list and
goals (team-charter.md:28-43). Section 2 explains how those three documents line
up. Section 3 holds the stories. Section 4 is the traceability table from each
story back to its use case and forward to the requirement it satisfies. Section
5 lists the assumptions and open questions the team still needs to settle.

Every story carries a "Source use case(s)" line so each one is traceable back to
the use-case artifact, which is in turn traceable back to the charter and the
requirements. This is the "Traceable" quality the template asks for
(user-story-template.md:47-48; Week 02 lecture, "Writing good requirements"
slide, p.17).

There are 25 stories (US-01 through US-25). This lands inside the 25 to 30 range
the course requires (requirements.md:149). Each story is one distinct
functionality. Edge cases and permission rules are captured as scenarios inside
the relevant story, not as separate stories, per the instructor Q&A
(requirements.md:377-379; user-story-template.md:50-57).

## 2. How the three documents align

This section synthesizes the team charter, the requirements, and the use cases
so all three agree before the stories are written. Where the documents differ,
the reconciliation is stated plainly.

### 2.1 Roles

All three documents agree on five roles. The charter In Scope list and the
requirements describe member, poster, claimant, and admin behavior
(requirements.md:33-35; team-charter.md:30-43). The use cases add Guest, the
person who holds an invite token but has not registered yet
(use-cases.md:42-51). The template names member, poster, claimant, and admin
today and notes guest, invited-user, and suspended-user may be added as the
role model grows (user-story-template.md:158-162).

Reconciled role set used in this document:

- Guest: holds an invite token but has not registered. Can only register.
- Member: a registered, active user. Can post, browse, search, claim, message,
  review, and view a dashboard.
- Poster: the Member who owns a specific listing. Poster and Claimant are not
  separate accounts; they are the role a Member plays for a given exchange
  (use-cases.md:37-40).
- Claimant: the Member who requests a quantity from someone else's listing.
- Admin: a privileged user who can suspend users, deactivate listings without
  deleting audit history, and generate basic reports.

### 2.2 The claim lifecycle

All three documents describe the same lifecycle, so there is no conflict to
resolve:

- REQUESTED -> APPROVED -> PICKED_UP -> COMPLETED is the main path.
- REQUESTED -> DENIED is a terminal rejection.
- CANCELLED is allowed only from REQUESTED or APPROVED. Cancellation after
  pickup is not allowed.
- Approved quantities must never exceed the remaining quantity (over-claim
  protection).

This matches requirements.md:33, team-charter.md:23,34, and the claim use cases
(use-cases.md:272-395). The business-rule scenarios in the stories below restate
these rules so each one maps to a test.

### 2.3 Scope reconciliation

The charter In Scope list (team-charter.md:30-43) and the use cases line up item
for item, with two points worth calling out:

1. Photo uploads. The charter lists "Photo uploads on listings" as its own In
   Scope item (team-charter.md:40), separate from creating and editing listings.
   The use cases fold photos into the create and edit use cases (UC-06, UC-07).
   To keep the charter's distinct item visible and testable, this document gives
   photos their own story (US-07) while still letting photos be added during
   create and edit. This is a distinct functionality, not a split of one feature
   across scenarios, so it is allowed under the instructor Q&A rule
   (requirements.md:377-379).

2. Email or SMS notifications. The charter lists this as a stretch goal
   (team-charter.md:43). The use-case artifact drops it from the use-case set and
   does not model it (use-cases.md:665). This document follows the use cases and
   does not write a story for email or SMS. In-app notifications are still
   covered (US-19). If the team wants email or SMS later, it becomes a new story
   through the change-request process (team-charter.md:107).

Everything else in the charter In Scope list maps cleanly to a story below. The
charter Out of Scope items (payments, public sign-up, native apps, delivery and
maps, live chat, recommendations, multi-language) are not turned into stories
(team-charter.md:46-54).

### 2.4 Story-to-use-case mapping at a glance

Most use cases become one story. Two use cases need a note:

- UC-06 and UC-07 (create and edit listings) contribute the photo behavior that
  becomes US-07, in addition to their own stories (US-06 and US-08).
- UC-14 (approve or deny a claim request) stays a single story (US-15). Approve
  and deny are two outcomes of one decision, so they are scenarios inside one
  story, not two stories. This respects the instructor warning against splitting
  one feature across goals or scenarios (requirements.md:150-153).

The full mapping is in the Section 4 traceability table.

### 2.5 Carried-forward assumptions

The use cases flag three working assumptions and one open report question
(use-cases.md:31-33,144-146,306,382,569-572). These carry into the stories and
are collected in Section 5. They are also noted inline in the affected stories
(US-04, US-14, US-17, US-25).

## 3. The user stories

Each story lists its source use case, priority, actor, and milestone, then the
story sentence, then its acceptance criteria as Gherkin scenarios. Scenarios
cover the normal path, and add edge or error and permission scenarios only where
the action needs them (user-story-template.md:116-123).

### 3.1 Authentication and account

#### US-01: Register with an invite token

- **Source use case:** UC-01 (Register with an invite token)
- **Priority:** high
- **Actor:** guest
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a guest, I want to register for an account using a valid invite
token so that I can join the trusted community.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the guest holds a valid, unused invite token
- And the guest provides complete account details
- When the guest submits the registration form with the token
- Then a member account is created
- And the invite token is marked as used

*Scenario 2 - edge / error path (bad token)*
- Given the guest holds a token that is invalid, expired, or already used
- When the guest submits the registration form with that token
- Then the system rejects the registration
- And no account is created

*Scenario 3 - edge / error path (bad details)*
- Given the guest holds a valid, unused invite token
- And a required account detail is missing or malformed
- When the guest submits the registration form
- Then the system rejects the submission
- And shows a validation error

**Notes / open questions / assumptions**
- Token expiry rules are not yet defined. Confirm whether tokens expire and after
  how long.

#### US-02: Log in

- **Source use case:** UC-02 (Log in)
- **Priority:** high
- **Actor:** member
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a member, I want to log in to my account so that I can reach member
features.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member has a registered account that is not suspended
- When the member submits correct credentials
- Then the system starts an authenticated session

*Scenario 2 - edge / error path (wrong credentials)*
- Given the member has a registered account
- When the member submits credentials that do not match
- Then the system denies login
- And no session is started

*Scenario 3 - permission rule (suspended account)*
- Given the member's account is suspended
- When the member submits correct credentials
- Then the system denies login
- And tells the member the account is suspended

**Notes / open questions / assumptions**
- None.

#### US-03: Log out

- **Source use case:** UC-03 (Log out)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a member, I want to log out so that no one else can use my account
on this device.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is logged in
- When the member chooses to log out
- Then the system ends the session
- And returns the member to the public view

*Scenario 2 - edge / error path (session already expired)*
- Given the member's session has already expired
- When the member chooses to log out
- Then the system returns the member to the public view
- And no error is shown

**Notes / open questions / assumptions**
- None.

#### US-04: Invite a new member

- **Source use case:** UC-04 (Invite a new member)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a member, I want to create and share an invite token so that a new
person can register and join the community.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is logged in and active
- And the member is allowed to issue invites
- When the member chooses to create an invite
- Then the system generates a new, unused invite token
- And shows the token so the member can share it

*Scenario 2 - permission rule (not allowed to invite)*
- Given the member is not allowed to issue invites
- When the member attempts to create an invite
- Then the system denies the action
- And no token is created

*Scenario 3 - permission rule (suspended member)*
- Given the member is suspended
- When the member attempts to create an invite
- Then the system denies the action
- And no token is created

**Notes / open questions / assumptions**
- ASSUMPTION (carried from use-cases.md:144-146): the source documents require
  invite-only registration but do not say who issues invite tokens. Confirm
  whether any active member can invite, or only an admin. If the team does not
  decide the issuer, keep US-01 as core and treat US-04 as optional or pending.

#### US-05: View and update member profile

- **Source use case:** UC-05 (Manage member profile)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a member, I want to view and update my profile details so that my
information stays current.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is logged in
- When the member edits a profile field and saves
- Then the system validates the change
- And saves the updated profile

*Scenario 2 - edge / error path (bad field)*
- Given the member is logged in
- And a profile field is missing or malformed
- When the member saves the change
- Then the system rejects the change
- And shows a validation error

*Scenario 3 - permission rule (editing another profile)*
- Given the member is logged in
- When the member attempts to edit another member's profile
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- None.

### 3.2 Listings (poster)

#### US-06: Create a listing

- **Source use case:** UC-06 (Create a listing)
- **Priority:** high
- **Actor:** poster
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a poster, I want to post a new listing with the required details so
that other members can find and claim the item.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the poster is a registered, active member
- When the poster enters a description, category, quantity available, dietary and
  allergen tags, and a pickup window, and saves
- Then the system validates the required details
- And saves the listing and makes it active

*Scenario 2 - edge / error path (missing detail)*
- Given the poster is creating a listing
- And a required detail is missing or invalid
- When the poster saves the listing
- Then the system rejects the listing
- And shows a validation error

*Scenario 3 - permission rule (suspended member)*
- Given the poster's account is suspended
- When the poster attempts to create a listing
- Then the system denies the action
- And no listing is created

**Notes / open questions / assumptions**
- Photo uploads during create are covered by US-07.

#### US-07: Add and manage listing photos

- **Source use case:** UC-06 (optional photos during create), UC-07 (add,
  replace, remove photos during edit)
- **Priority:** low
- **Actor:** poster
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a poster, I want to add, replace, and remove photos on my listing
so that claimants can see what the item looks like.

**Acceptance criteria**

*Scenario 1 - normal / valid path (add during create or edit)*
- Given the poster owns the listing
- When the poster adds one or more photos and saves
- Then the system stores the photos with the listing

*Scenario 2 - normal / valid path (replace or remove)*
- Given the poster owns a listing that already has photos
- When the poster replaces or removes a photo and saves
- Then the system updates the listing's photos

*Scenario 3 - permission rule (not the owner)*
- Given the listing belongs to another member
- When the member attempts to change its photos
- Then the system denies the action
- And the photos do not change

**Notes / open questions / assumptions**
- Photos are optional on a listing (requirements.md:33). Confirm allowed file
  types and a maximum size or count before building.

#### US-08: Edit a listing

- **Source use case:** UC-07 (Edit a listing)
- **Priority:** medium
- **Actor:** poster
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a poster, I want to update the details of one of my listings so
that the information stays accurate.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the poster owns the listing
- When the poster changes the details and saves
- Then the system validates the changes
- And saves the updated listing

*Scenario 2 - edge / error path (bad detail)*
- Given the poster owns the listing
- And a changed detail is missing or invalid
- When the poster saves the change
- Then the system rejects the change
- And shows a validation error

*Scenario 3 - permission rule (not the owner)*
- Given the listing belongs to another member
- When the member attempts to edit it
- Then the system denies the action
- And nothing changes

*Scenario 4 - permission rule (suspended member)*
- Given the poster's account is suspended
- When the poster attempts to edit the listing
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- Editing photos is covered by US-07.

#### US-09: Deactivate own listing

- **Source use case:** UC-08 (Deactivate own listing)
- **Priority:** medium
- **Actor:** poster
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a poster, I want to deactivate one of my listings so that no new
claims can be made on it.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the poster owns an active listing
- When the poster chooses to deactivate it
- Then the system marks the listing inactive
- And hides it from browsing

*Scenario 2 - permission rule (not the owner)*
- Given the listing belongs to another member
- When the member attempts to deactivate it
- Then the system denies the action
- And the listing stays active

**Notes / open questions / assumptions**
- Confirm what happens to claims already in progress on a listing that is
  deactivated. Assumption: existing claims continue through their lifecycle;
  only new claims are blocked.

### 3.3 Discovery (member)

#### US-10: Browse, search, and filter active listings

- **Source use case:** UC-09 (Browse, search, and filter active listings)
- **Priority:** high
- **Actor:** member
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a member, I want to browse, search, and filter active listings so
that I can find items of interest.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is logged in
- When the member enters search terms or chooses filters such as category or
  dietary and allergen tags
- Then the system returns the active listings that match

*Scenario 2 - edge / error path (no matches)*
- Given the member is logged in
- When no active listings match the search or filters
- Then the system shows an empty result

**Notes / open questions / assumptions**
- Confirm the exact filter set. At minimum: category and dietary and allergen
  tags (requirements.md:33).

#### US-11: View listing details

- **Source use case:** UC-10 (View listing details)
- **Priority:** high
- **Actor:** member
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a member, I want to see the full details of one listing so that I
can decide whether to claim it.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is logged in
- And the listing is active
- When the member selects the listing
- Then the system shows its full details, including any photos, the quantity
  available, the tags, and the pickup window

*Scenario 2 - edge / error path (no longer active)*
- Given the member selects a listing that is no longer active
- When the system loads the listing
- Then the system tells the member the listing is unavailable

**Notes / open questions / assumptions**
- None.

### 3.4 Claims (claimant side)

#### US-12: Submit a claim for a quantity of a listing

- **Source use case:** UC-11 (Claim produce from a listing)
- **Priority:** high
- **Actor:** claimant
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a claimant, I want to submit a claim for a specified quantity of a
listing so that I can request only the amount I need.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given a listing is active
- And the listing has 10 units available
- When the claimant requests 3 units
- Then a claim is created with status REQUESTED
- And the requested quantity is 3
- And the poster is notified of the new request

*Scenario 2 - edge / error path (quantity exceeds available)*
- Given a listing is active
- And the listing has 10 units available
- When the claimant requests 12 units
- Then the system rejects the claim
- And no claim is created

*Scenario 3 - edge / error path (quantity must be positive)*
- Given a listing is active
- When the claimant requests 0 units
- Then the system rejects the claim
- And shows a validation error

*Scenario 4 - permission rule (suspended member)*
- Given the user submitting the request is suspended
- When the suspended user attempts to submit a claim
- Then the system denies access
- And no claim is created

**Notes / open questions / assumptions**
- This story models the requirements Sample User Story #1
  (requirements.md:43-67).

#### US-13: Cancel a claim

- **Source use case:** UC-13 (Cancel a claim)
- **Priority:** medium
- **Actor:** claimant
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a claimant, I want to cancel a claim that I no longer need so that
the poster knows the item is free again.

**Acceptance criteria**

*Scenario 1 - normal / valid path (from REQUESTED or APPROVED)*
- Given the claimant owns a claim with status REQUESTED or APPROVED
- When the claimant chooses to cancel it
- Then the system changes the claim status to CANCELLED
- And the poster is notified

*Scenario 2 - workflow rule (no cancellation after pickup)*
- Given the claim has status PICKED_UP or COMPLETED
- When the claimant attempts to cancel it
- Then the system rejects the cancellation
- And the status does not change

*Scenario 3 - permission rule (not the claimant)*
- Given a member who is not the claimant for the claim
- When that member attempts to cancel it
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- CANCELLED is terminal (use-cases.md:342).

#### US-14: Confirm pickup

- **Source use case:** UC-12 (Confirm pickup)
- **Priority:** high
- **Actor:** claimant
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a claimant, I want to confirm that I picked up the item so that the
exchange can move toward completion.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the claimant owns a claim with status APPROVED
- When the claimant confirms pickup
- Then the system changes the claim status from APPROVED to PICKED_UP
- And the poster is notified

*Scenario 2 - workflow rule (wrong status)*
- Given the claim is not in APPROVED status
- When the claimant attempts to confirm pickup
- Then the system rejects the action
- And nothing changes

*Scenario 3 - permission rule (not the claimant)*
- Given a member who is not the claimant for the claim
- When that member attempts to confirm pickup
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- ASSUMPTION (carried from use-cases.md:306): the claimant owns the move to
  PICKED_UP. Confirm this with the team.

### 3.5 Claims (poster side)

#### US-15: Approve or deny a claim request

- **Source use case:** UC-14 (Approve or deny a claim request)
- **Priority:** high
- **Actor:** poster
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a poster, I want to approve or deny a pending claim on my listing
so that I control who receives my items and how much they get.

**Acceptance criteria**

*Scenario 1 - normal / valid path (approve)*
- Given the poster owns the listing
- And the claim has status REQUESTED
- And the approved quantity will not exceed the remaining quantity
- When the poster approves the claim
- Then the system changes the claim status to APPROVED
- And the claimant is notified

*Scenario 2 - normal / valid path (deny)*
- Given the poster owns the listing
- And the claim has status REQUESTED
- When the poster denies the claim
- Then the system changes the claim status to DENIED
- And the claimant is notified

*Scenario 3 - workflow rule (over-claim protection)*
- Given the poster owns the listing
- And approving this claim would push the cumulative approved quantity past the
  remaining quantity
- When the poster attempts to approve it
- Then the system rejects the approval
- And the status does not change

*Scenario 4 - workflow rule (wrong status)*
- Given the claim is not in REQUESTED status
- When the poster attempts to approve or deny it
- Then the system rejects the action
- And nothing changes

*Scenario 5 - permission rule (not the listing owner)*
- Given a member who does not own the listing
- When that member attempts to approve or deny the claim
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- DENIED is terminal (use-cases.md:371). Approve and deny are two outcomes of one
  decision, so they are scenarios in this one story, not separate stories.

#### US-16: Complete an exchange

- **Source use case:** UC-15 (Complete an exchange)
- **Priority:** high
- **Actor:** poster
- **Milestone:** Sprint 1 - Core exchange flow (R1)

**Story:** As a poster, I want to mark a picked-up exchange as completed so that
both parties can leave a review.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the poster owns the listing
- And the claim has status PICKED_UP
- When the poster marks the exchange complete
- Then the system changes the claim status from PICKED_UP to COMPLETED
- And the claimant is notified

*Scenario 2 - workflow rule (wrong status)*
- Given the claim is not in PICKED_UP status
- When the poster attempts to mark the exchange complete
- Then the system rejects the action
- And nothing changes

*Scenario 3 - permission rule (not the listing owner)*
- Given a member who does not own the listing
- When that member attempts to complete the exchange
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- ASSUMPTION (carried from use-cases.md:382): the poster owns the move to
  COMPLETED. Confirm this with the team.

### 3.6 Coordination and trust

#### US-17: Coordinate pickup via the exchange message thread

- **Source use case:** UC-16 (Coordinate pickup via the exchange message thread)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a member who is a party to an exchange, I want to send and read
messages in that exchange's private thread so that we can arrange the pickup.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is the poster or the claimant for the exchange
- When the member writes and sends a message
- Then the system saves the message to the thread
- And shows it to both parties

*Scenario 2 - edge / error path (empty message)*
- Given the member is a party to the exchange
- When the member sends an empty message
- Then the system rejects it

*Scenario 3 - permission rule (not a party)*
- Given a member who is not part of the exchange
- When that member attempts to open the thread
- Then the system denies access

**Notes / open questions / assumptions**
- One thread per exchange (requirements.md:33).

#### US-18: View status notifications

- **Source use case:** UC-17 (View status notifications). Notifications are
  generated by UC-11, UC-12, UC-13, UC-14, and UC-15, which map to US-12, US-14,
  US-13, US-15, and US-16.
- **Priority:** medium
- **Actor:** member
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a member, I want to open and read in-app notifications about
exchange status changes so that I know when I need to act.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is logged in
- And the member has notifications
- When the member opens their notifications
- Then the system shows them newest first
- And the member can open the related exchange

*Scenario 2 - edge / error path (none)*
- Given the member has no notifications
- When the member opens their notifications
- Then the system shows an empty list

**Notes / open questions / assumptions**
- Notifications are generated as a step inside the claim stories (US-12, US-13,
  US-14, US-15, US-16). This story is only the member viewing them
  (use-cases.md:436-439).
- Email or SMS notifications are out of scope for this story set
  (use-cases.md:665; team-charter.md:43).

#### US-19: Leave a rating and review after completion

- **Source use case:** UC-18 (Leave a rating and review after completion)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a member who took part in a completed exchange, I want to leave a
short rating and review of the other party so that the community can build trust.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given an exchange has status COMPLETED
- And the member is the poster or the claimant for that exchange
- And the member has not already reviewed the other party
- When the member submits a rating and a short review
- Then the system saves the review
- And links it to the completed exchange
- And makes it visible to the reviewed party

*Scenario 2 - workflow rule (not completed)*
- Given an exchange has status REQUESTED, APPROVED, PICKED_UP, DENIED, or
  CANCELLED
- When the member attempts to submit a review
- Then the system rejects the review
- And no review is saved

*Scenario 3 - edge / error path (duplicate review)*
- Given the member has already reviewed the other party for this exchange
- When the member attempts to review again
- Then the system rejects the duplicate review

*Scenario 4 - permission rule (non-participant)*
- Given an exchange has status COMPLETED
- And the member is not the poster or claimant for that exchange
- When the member attempts to submit a review
- Then the system denies access
- And no review is saved

**Notes / open questions / assumptions**
- This story models the requirements Sample User Story #2
  (requirements.md:69-104).

#### US-20: View reviews for a completed exchange

- **Source use case:** UC-19 (View reviews for a completed exchange)
- **Priority:** low
- **Actor:** member
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a member, I want to view the reviews left for a completed exchange
so that I can see feedback, including any review left about me.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given an exchange has status COMPLETED
- And the member is the poster or the claimant for that exchange
- When the member opens the reviews for that exchange
- Then the system shows the reviews linked to it, including any review left about
  the viewing member

*Scenario 2 - edge / error path (no reviews yet)*
- Given a completed exchange has no reviews yet
- When a participant opens the reviews
- Then the system shows that there are no reviews yet

*Scenario 3 - permission rule (non-participant)*
- Given a member who is not a participant in the exchange
- When that member attempts to view the reviews
- Then the system denies access

**Notes / open questions / assumptions**
- None.

#### US-21: View my dashboard and activity overview

- **Source use case:** UC-20 (View my dashboard and activity overview)
- **Priority:** high
- **Actor:** member
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As a member, I want a single dashboard of my activity so that I can see
my active listings, incoming and outgoing requests, and exchange history in one
place.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is logged in
- When the member opens their dashboard
- Then the system shows their active listings, incoming requests, outgoing
  requests, and exchange history, grouped by status
- And each status-based action links to its own feature (deactivate a listing,
  confirm pickup, cancel a claim, approve or deny a request, complete an
  exchange)

*Scenario 2 - edge / error path (no activity)*
- Given the member has no activity yet
- When the member opens their dashboard
- Then the system shows empty groups

**Notes / open questions / assumptions**
- The dashboard only views activity. The status-based actions live in their own
  stories: US-09, US-14, US-13, US-15, and US-16 (use-cases.md:507-512).

### 3.7 Admin

#### US-22: Suspend a user

- **Source use case:** UC-21 (Suspend a user)
- **Priority:** medium
- **Actor:** admin
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As an admin, I want to suspend a member account so that a misbehaving
user can no longer log in or take member actions.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the admin is logged in with admin rights
- And the target account exists
- When the admin suspends the account
- Then the system marks the account suspended
- And the account can no longer log in or take member actions

*Scenario 2 - permission rule (non-admin)*
- Given a user without admin rights
- When that user attempts to suspend an account
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- The suspended state is referenced by US-02, US-04, US-06, US-08, and US-12.
  Confirm whether suspension can be reversed; reinstatement is not yet a story.

#### US-23: Deactivate a listing as admin

- **Source use case:** UC-22 (Deactivate a listing as admin)
- **Priority:** medium
- **Actor:** admin
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As an admin, I want to deactivate any listing so that I can hide it
from browsing without deleting its audit history.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the admin is logged in with admin rights
- And the listing exists
- When the admin deactivates the listing
- Then the system hides the listing from browsing
- And keeps its audit history

*Scenario 2 - permission rule (non-admin)*
- Given a user without admin rights
- When that user attempts to deactivate a listing as admin
- Then the system denies the action
- And nothing changes

**Notes / open questions / assumptions**
- None.

#### US-24: Generate basic reports

- **Source use case:** UC-23 (Generate basic reports)
- **Priority:** low
- **Actor:** admin
- **Milestone:** Sprint 2 - Full features (R2)

**Story:** As an admin, I want to generate a basic report about system activity so
that I can see how the exchange is being used.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the admin is logged in with admin rights
- When the admin generates a report
- Then the system computes the report
- And shows it to the admin

*Scenario 2 - permission rule (non-admin)*
- Given a user without admin rights
- When that user attempts to generate a report
- Then the system denies the action

**Notes / open questions / assumptions**
- ASSUMPTION (carried from use-cases.md:569-572): to keep this verifiable, assume
  at least one concrete report type, an active-listing count and a
  completed-exchange count. The full set of report types is an open question for
  the cross-team review packet.

### 3.8 Stretch

#### US-25: View pickup reminders (stretch)

- **Source use case:** UC-24 (View pickup reminders, stretch)
- **Priority:** low
- **Actor:** member
- **Milestone:** Stretch - if time allows

**Story:** As a member, I want to view reminders that a pickup window is getting
close so that I do not miss an exchange.

**Acceptance criteria**

*Scenario 1 - normal / valid path*
- Given the member is a party to an exchange with an upcoming pickup window
- When the member opens their reminders
- Then the system shows reminders for exchanges whose pickup window is getting
  close

*Scenario 2 - edge / error path (none)*
- Given the member has no upcoming pickups
- When the member opens their reminders
- Then the system shows an empty list

**Notes / open questions / assumptions**
- This is a stretch story (team-charter.md:42). Build it only if time allows.
- The reminder is generated by the system. The member goal here is viewing
  reminders, not receiving them (use-cases.md:596-597).

Note on numbering: this document has 25 stories, US-01 through US-25. The Section
4 table below is the authoritative list. If the team wants a 26th story to widen
the margin inside the 25 to 30 range, the natural option is to split US-15 into a
separate approve story and deny story. This document keeps them together as one
story, with approve and deny as scenarios, per the instructor Q&A
(requirements.md:150-153,377-379).

## 4. Traceability table

This table is the authoritative story list. It traces each story back to its
source use case and forward to the requirement or charter item it satisfies.
This gives two-way traceability: charter and requirements -> use cases -> user
stories (user-story-template.md:47-48).

| Story ID | Title | Source use case | Requirement / charter source |
| --- | --- | --- | --- |
| US-01 | Register with an invite token | UC-01 | requirements.md:33 (join only via invitation); team-charter.md:30 |
| US-02 | Log in | UC-02 | team-charter.md:30 |
| US-03 | Log out | UC-03 | team-charter.md:30 |
| US-04 | Invite a new member | UC-04 | requirements.md:33 (invite-only); team-charter.md:30 (assumption) |
| US-05 | View and update member profile | UC-05 | requirements.md:33,35; team-charter.md:30 |
| US-06 | Create a listing | UC-06 | requirements.md:33; team-charter.md:31 |
| US-07 | Add and manage listing photos | UC-06, UC-07 | requirements.md:33; team-charter.md:40 |
| US-08 | Edit a listing | UC-07 | team-charter.md:31 |
| US-09 | Deactivate own listing | UC-08 | team-charter.md:31 |
| US-10 | Browse, search, and filter active listings | UC-09 | requirements.md:33; team-charter.md:32 |
| US-11 | View listing details | UC-10 | requirements.md:33 |
| US-12 | Submit a claim for a quantity | UC-11 | requirements.md:33,45-67; team-charter.md:33 |
| US-13 | Cancel a claim | UC-13 | requirements.md:33 (CANCELLED rule); team-charter.md:34 |
| US-14 | Confirm pickup | UC-12 | requirements.md:33 (APPROVED -> PICKED_UP) |
| US-15 | Approve or deny a claim request | UC-14 | requirements.md:33 (REQUESTED -> APPROVED / DENIED; over-claim) |
| US-16 | Complete an exchange | UC-15 | requirements.md:33 (PICKED_UP -> COMPLETED) |
| US-17 | Coordinate pickup via the exchange message thread | UC-16 | requirements.md:33; team-charter.md:35 |
| US-18 | View status notifications | UC-17 | requirements.md:33; team-charter.md:41 |
| US-19 | Leave a rating and review after completion | UC-18 | requirements.md:33,71-104; team-charter.md:36 |
| US-20 | View reviews for a completed exchange | UC-19 | requirements.md:33,82,91 |
| US-21 | View my dashboard and activity overview | UC-20 | requirements.md:35; team-charter.md:37 |
| US-22 | Suspend a user | UC-21 | requirements.md:35; team-charter.md:38 |
| US-23 | Deactivate a listing as admin | UC-22 | requirements.md:35; team-charter.md:38 |
| US-24 | Generate basic reports | UC-23 | requirements.md:35; team-charter.md:38 |
| US-25 | View pickup reminders (stretch) | UC-24 | requirements.md:33; team-charter.md:42 |

### 4.1 Use-case coverage check

Every use case in use-cases.md maps to at least one story. UC-01 through UC-24
are all covered. UC-24 is covered by the stretch story US-25. The photo behavior
inside UC-06 and UC-07 is covered by US-07 in addition to US-06 and US-08. No use
case is left out.

### 4.2 Role coverage check

Every role is the actor of at least one story (user-story-template.md and
use-cases.md:630-643):

| Role | Stories where this role is the actor |
| --- | --- |
| Guest | US-01 |
| Member | US-02, US-03, US-04, US-05, US-10, US-11, US-17, US-18, US-19, US-20, US-21, US-25 |
| Poster | US-06, US-07, US-08, US-09, US-15, US-16 |
| Claimant | US-12, US-13, US-14 |
| Admin | US-22, US-23, US-24 |

### 4.3 Business-rule coverage check

| Business rule (requirements.md:33) | Covered by scenario |
| --- | --- |
| Request cannot exceed total quantity available | US-12 Scenario 2 |
| Claim quantity must be positive | US-12 Scenario 3 |
| REQUESTED -> APPROVED | US-15 Scenario 1 |
| REQUESTED -> DENIED (terminal) | US-15 Scenario 2 |
| Approved quantities never exceed remaining quantity (over-claim) | US-15 Scenario 3 |
| APPROVED -> PICKED_UP | US-14 Scenario 1 |
| PICKED_UP -> COMPLETED | US-16 Scenario 1 |
| CANCELLED only from REQUESTED or APPROVED | US-13 Scenario 1 |
| No cancellation after pickup | US-13 Scenario 2 |
| Review only after completion | US-19 Scenario 2 |
| Non-participant cannot review | US-19 Scenario 4 |

## 5. Assumptions and open questions

These items are not yet approved by the team. Each one should be confirmed or
changed before the requirements are locked (requirements.md:332;
use-cases.md:698-706). They are also noted inline in the affected stories.

| ID | Item | Affected stories | Status |
| --- | --- | --- | --- |
| A1 | Who may issue invite tokens (any active member, or admin only) | US-04 | Open |
| A2 | The claimant owns the move to PICKED_UP | US-14 | Assumed |
| A3 | The poster owns the move to COMPLETED | US-16 | Assumed |
| A4 | Report types (assume active-listing count and completed-exchange count) | US-24 | Assumed |
| A5 | Do invite tokens expire, and after how long | US-01 | Open |
| A6 | Allowed photo file types, size, and count | US-07 | Open |
| A7 | What happens to in-progress claims when a listing is deactivated | US-09 | Assumed (existing claims continue) |
| A8 | Whether suspension can be reversed (reinstatement) | US-22 | Open |

## 6. INVEST and Definition of Ready note

Each story above was checked against the INVEST criteria
(user-story-template.md:128-143; Week 02 lecture, "INVEST criteria for user
stories" slide, p.13). Each one is independent enough to build on its own, states
clear value in its "so that" clause, is small enough for a single sprint, and has
testable acceptance criteria. Stories with open assumptions in Section 5 are not
yet "Ready" in the Definition of Ready sense (user-story-template.md:179-184)
until those assumptions are confirmed. Each story will become one GitHub Issue
with Priority, Type: story, Actor, and Milestone labels, then be added to the
project board (user-story-template.md:146-168).
