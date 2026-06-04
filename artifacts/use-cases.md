# High-level Use Cases: Local Produce Exchange

## 1. Introduction

This document is the "High-level use cases" artifact for the Local Produce
Exchange. It is one of the Core Requirements for the inception deliverable due
June 8 (requirements.md:148). It lists the complete set of use cases the team
plans to build, written at a high level so each one states a single actor goal
and how that goal ends. Deep detail such as exact inputs, every branch, and
validation messages is left out on purpose. That detail belongs in the 25 to 30
user stories and their acceptance criteria, which are the next deliverable
(requirements.md:149; use-case-template.md:29-31).

Each use case follows the exact template block from
use-case-template.md:60-79. The set is drawn from three sources: the team
charter In Scope list and goals (team-charter.md:28-43), the project
requirements including the claim lifecycle and business rules
(requirements.md:33-35), and the Project Initiation Outline (Slides 4, 6, and
7).

The roles used here are Guest, Member, Poster, Claimant, and Admin. They are
defined in Section 2.

A note on numbering for reviewers: the use-case IDs in this document (UC-01
through UC-24) are numbered independently of the template's illustrative worked
example, which uses UC-04 for "Claim produce from a listing"
(use-case-template.md:147,204). In this document UC-04 is "Invite a new member"
and the claim use case is UC-11. Reviewers should not expect the numbers to
match the template's example.

This artifact also carries three working assumptions that the team has not yet
approved. They are flagged inline at the affected use cases and collected in the
assumptions and open questions table in Section 6.

## 2. Actors and Roles

These are the roles the system defines. Poster and Claimant are not separate
accounts. They are the role a Member plays for a given exchange. The same person
can be a Poster on one listing and a Claimant on another
(requirements.md:33-35,341; Initiation Outline Slide 6).

- Guest: a person who holds an invite token but has not registered yet. A Guest
  can only register.
- Member: a registered, active user. A Member can post listings, browse, search,
  submit claims, send messages, leave reviews, and view a personal dashboard.
- Poster: the Member who owns a specific listing. A Poster approves or denies
  claims on that listing and marks the exchange complete.
- Claimant: the Member who requests a quantity from someone else's listing. A
  Claimant submits, cancels, and confirms pickup for that claim.
- Admin: a privileged user who can suspend users, deactivate listings without
  deleting audit history, and generate basic reports.

## 3. Use Cases

The use cases below are grouped by topic. Each numbered subsection (3.1 through
3.8) is one topic area, and the use cases inside it share that theme. The eight
topic groups are: Authentication and account, Listings, Discovery, Claims
(claimant side), Claims (poster side), Coordination and trust, Admin, and Stretch
use cases.

### 3.1 Authentication and account

#### Use Case UC-01: Register with an invite token

- Primary actor: Guest
- Supporting actors: System (validates the token)
- Goal: Create a member account using a valid invite token so the Guest can join
  the community.
- Preconditions: The Guest holds an invite token. The token is valid and has not
  been used.
- Trigger: The Guest opens the registration page and submits account details
  with the token.
- Main success flow:
  1. The Guest opens the registration page.
  2. The Guest enters account details and the invite token.
  3. The system checks that the token is valid and unused.
  4. The system creates the member account.
  5. The system marks the token as used.
- Alternate and exception flows:
  - Token is invalid, expired, or already used: the system rejects registration
    and creates no account.
  - Required account details are missing or malformed: the system rejects the
    submission and shows a validation error.
- Postconditions: A member account exists and the invite token is marked as
  used.
- Related user stories: TBD.

#### Use Case UC-02: Log in

- Primary actor: Member
- Supporting actors: System (checks credentials)
- Goal: Sign in to a member account to reach member features.
- Preconditions: The Member has a registered account that is not suspended.
- Trigger: The Member submits login credentials.
- Main success flow:
  1. The Member opens the login page.
  2. The Member enters credentials.
  3. The system checks the credentials.
  4. The system starts an authenticated session.
- Alternate and exception flows:
  - Credentials do not match: the system denies login and starts no session.
  - The account is suspended: the system denies login and tells the Member the
    account is suspended.
- Postconditions: The Member has an active authenticated session.
- Related user stories: TBD.

#### Use Case UC-03: Log out

- Primary actor: Member
- Supporting actors: System (ends the session)
- Goal: End the current session so no one else can use the account on this
  device.
- Preconditions: The Member is logged in.
- Trigger: The Member chooses to log out.
- Main success flow:
  1. The Member chooses to log out.
  2. The system ends the authenticated session.
  3. The system returns the Member to the public view.
- Alternate and exception flows:
  - The session has already expired: the system simply returns the Member to the
    public view.
- Postconditions: The Member has no active session.
- Related user stories: TBD.

#### Use Case UC-04: Invite a new member

- Primary actor: Member
- Supporting actors: System (creates the invite token)
- Goal: Create and share an invite token so a new person can register.
- Preconditions: The Member is logged in and active. (Assumption: a Member may
  issue invite tokens. The exact issuer is an open question. See Section 6.)
- Trigger: The Member chooses to invite a new person.
- Main success flow:
  1. The Member chooses to create an invite.
  2. The system generates a new, unused invite token.
  3. The system shows the token so the Member can share it.
- Alternate and exception flows:
  - The issuer is not allowed to invite (pending the open question in Section 6):
    the system denies the action and creates no token.
  - The member is suspended: the system denies the action and creates no token.
- Postconditions: A new, unused invite token exists.
- Related user stories: TBD.

Note: This use case is an assumption. The source files require invite-only
registration but do not say who issues invite tokens. If the team does not
decide the issuer, keep UC-01 as core and treat UC-04 as optional or pending.

#### Use Case UC-05: Manage member profile

- Primary actor: Member
- Supporting actors: System (saves the profile)
- Goal: View and update personal profile details.
- Preconditions: The Member is logged in.
- Trigger: The Member opens the profile page and edits details.
- Main success flow:
  1. The Member opens the profile page.
  2. The Member edits profile details.
  3. The system validates the changes.
  4. The system saves the updated profile.
- Alternate and exception flows:
  - A profile field is missing or malformed: the system rejects the change and
    shows a validation error.
  - A Member tries to edit another member's profile: the system denies the
    action.
- Postconditions: The Member's profile reflects the saved changes.
- Related user stories: TBD.

### 3.2 Listings (Poster)

#### Use Case UC-06: Create a listing

- Primary actor: Poster
- Supporting actors: System (saves the listing)
- Goal: Post a new listing with the required details so other members can claim
  the item.
- Preconditions: The Poster is a registered, active member.
- Trigger: The Poster chooses to create a listing.
- Main success flow:
  1. The Poster chooses to create a listing.
  2. The Poster enters the description, category, quantity available, dietary and
     allergen tags, and a pickup window.
  3. The system validates the required details.
  4. The system saves the listing and makes it active.
- Alternate and exception flows:
  - A required detail is missing or invalid: the system rejects the listing and
    shows a validation error.
  - The Poster also adds one or more optional photos: the system stores the
    photos with the listing.
  - The member is suspended: the system denies the action and creates no
    listing.
- Postconditions: A new active listing exists with the entered details.
- Related user stories: TBD.

#### Use Case UC-07: Edit a listing

- Primary actor: Poster
- Supporting actors: System (saves the changes)
- Goal: Update the details of an existing listing, including its photos.
- Preconditions: The Poster owns the listing.
- Trigger: The Poster chooses to edit one of their listings.
- Main success flow:
  1. The Poster opens one of their listings to edit.
  2. The Poster changes the details.
  3. The system validates the changes.
  4. The system saves the updated listing.
- Alternate and exception flows:
  - A changed detail is missing or invalid: the system rejects the change and
    shows a validation error.
  - The Poster adds, replaces, or removes optional photos: the system updates the
    listing's photos.
  - A Member tries to edit a listing they do not own: the system denies the
    action.
  - The member is suspended: the system denies the action and saves no change.
- Postconditions: The listing reflects the saved changes.
- Related user stories: TBD.

#### Use Case UC-08: Deactivate own listing

- Primary actor: Poster
- Supporting actors: System (hides the listing)
- Goal: Take a listing out of browsing so no new claims can be made on it.
- Preconditions: The Poster owns the listing and the listing is active.
- Trigger: The Poster chooses to deactivate one of their listings.
- Main success flow:
  1. The Poster opens one of their active listings.
  2. The Poster chooses to deactivate it.
  3. The system marks the listing inactive and hides it from browsing.
- Alternate and exception flows:
  - A Member tries to deactivate a listing they do not own: the system denies the
    action.
- Postconditions: The listing is inactive and no longer appears in browsing.
- Related user stories: TBD.

### 3.3 Discovery (Member)

#### Use Case UC-09: Browse, search, and filter active listings

- Primary actor: Member
- Supporting actors: System (returns matching listings)
- Goal: Find active listings of interest by browsing, searching, and filtering.
- Preconditions: The Member is logged in.
- Trigger: The Member opens the listings view or enters search and filter terms.
- Main success flow:
  1. The Member opens the listings view.
  2. The Member enters search terms or chooses filters such as category or
     dietary and allergen tags.
  3. The system returns the active listings that match.
- Alternate and exception flows:
  - No listings match: the system shows an empty result.
- Postconditions: The Member sees the set of active listings that match.
- Related user stories: TBD.

#### Use Case UC-10: View listing details

- Primary actor: Member
- Supporting actors: System (returns the listing)
- Goal: See the full details of one listing before deciding to claim it.
- Preconditions: The Member is logged in and the listing is active.
- Trigger: The Member selects a listing.
- Main success flow:
  1. The Member selects a listing.
  2. The system shows the listing's full details, including any photos, the
     quantity available, the tags, and the pickup window.
- Alternate and exception flows:
  - The listing is no longer active: the system tells the Member the listing is
    unavailable.
- Postconditions: The Member has seen the listing's full details.
- Related user stories: TBD.

### 3.4 Claims (Claimant side)

#### Use Case UC-11: Claim produce from a listing

- Primary actor: Claimant
- Supporting actors: Poster (owns the listing); System (checks quantity, notifies
  the Poster)
- Goal: Request a specific quantity from an active listing so the Claimant
  receives only the amount they need.
- Preconditions: The Claimant is a registered, active member. The listing is
  active and has a quantity available.
- Trigger: The Claimant opens an active listing and chooses to request it.
- Main success flow:
  1. The Claimant opens an active listing.
  2. The Claimant enters the quantity they want.
  3. The system checks the amount against the total quantity available.
  4. The system creates a claim with status REQUESTED.
  5. The system notifies the Poster of the new request.
- Alternate and exception flows:
  - Requested amount is more than the total quantity available: the system
    rejects the request and creates no claim (requirements.md:56-61).
  - Requested amount is zero or negative: the system rejects it and shows a
    validation error (requirements.md:63-67).
  - The member is suspended: the system denies access and creates no claim.
- Postconditions: A claim exists with status REQUESTED for the requested amount,
  and the Poster has been notified.
- Related user stories: TBD.

#### Use Case UC-12: Confirm pickup

- Primary actor: Claimant
- Supporting actors: Poster (the other party); System (updates status, notifies
  the Poster)
- Goal: Confirm that the Claimant has picked up the item so the exchange can move
  toward completion.
- Preconditions: The claim has status APPROVED and the Claimant owns the claim.
  (Assumption: the Claimant owns the move to PICKED_UP. See Section 6.)
- Trigger: The Claimant chooses to confirm pickup.
- Main success flow:
  1. The Claimant opens their approved claim.
  2. The Claimant chooses to confirm pickup.
  3. The system changes the claim status from APPROVED to PICKED_UP.
  4. The system notifies the Poster.
- Alternate and exception flows:
  - The claim is not in APPROVED status: the system rejects the action and
    changes nothing.
  - A Member who is not the Claimant tries to confirm pickup: the system denies
    the action.
- Postconditions: The claim has status PICKED_UP and the Poster has been
  notified.
- Related user stories: TBD.

#### Use Case UC-13: Cancel a claim

- Primary actor: Claimant
- Supporting actors: Poster (the other party); System (updates status, notifies
  the Poster)
- Goal: Cancel a claim that is no longer needed before pickup happens.
- Preconditions: The claim has status REQUESTED or APPROVED and the Claimant owns
  the claim.
- Trigger: The Claimant chooses to cancel the claim.
- Main success flow:
  1. The Claimant opens their claim.
  2. The Claimant chooses to cancel it.
  3. The system changes the claim status to CANCELLED.
  4. The system notifies the Poster.
- Alternate and exception flows:
  - The claim has status PICKED_UP or COMPLETED: the system rejects the
    cancellation, because cancellation after pickup is not allowed
    (requirements.md:33).
  - A Member who is not the Claimant tries to cancel: the system denies the
    action.
- Postconditions: The claim has status CANCELLED, which is terminal, and the
  Poster has been notified.
- Related user stories: TBD.

### 3.5 Claims (Poster side)

#### Use Case UC-14: Approve or deny a claim request

- Primary actor: Poster
- Supporting actors: Claimant (the other party); System (checks quantity,
  notifies the Claimant)
- Goal: Decide a pending claim request on the Poster's own listing by approving
  or denying it.
- Preconditions: The Poster owns the listing. The claim has status REQUESTED.
- Trigger: The Poster opens a pending request on one of their listings.
- Main success flow:
  1. The Poster opens a pending request.
  2. The Poster chooses to approve or deny it.
  3. If approving, the system checks that the cumulative approved quantity does
     not exceed the remaining quantity.
  4. The system changes the claim status to APPROVED or DENIED.
  5. The system notifies the Claimant.
- Alternate and exception flows:
  - Approving would push the cumulative approved quantity past the remaining
    quantity when several claims compete: the system rejects the approval
    (requirements.md:33).
  - A Member who does not own the listing tries to approve or deny: the system
    denies the action.
  - The claim is not in REQUESTED status: the system rejects the action.
- Postconditions: The claim has status APPROVED or DENIED. DENIED is terminal.
  The Claimant has been notified.
- Related user stories: TBD.

#### Use Case UC-15: Complete an exchange

- Primary actor: Poster
- Supporting actors: Claimant (the other party); System (updates status, notifies
  the Claimant)
- Goal: Mark a picked-up exchange as completed so both parties can review it.
- Preconditions: The claim has status PICKED_UP and the Poster owns the listing.
  (Assumption: the Poster owns the move to COMPLETED. See Section 6.)
- Trigger: The Poster chooses to mark the exchange complete.
- Main success flow:
  1. The Poster opens the picked-up exchange.
  2. The Poster chooses to mark it complete.
  3. The system changes the claim status from PICKED_UP to COMPLETED.
  4. The system notifies the Claimant.
- Alternate and exception flows:
  - The claim is not in PICKED_UP status: the system rejects the action.
  - A Member who does not own the listing tries to complete the exchange: the
    system denies the action.
- Postconditions: The claim has status COMPLETED and the Claimant has been
  notified.
- Related user stories: TBD.

### 3.6 Coordination and trust

#### Use Case UC-16: Coordinate pickup via the exchange message thread

- Primary actor: Member (either party in the exchange)
- Supporting actors: System (stores and delivers the message)
- Goal: Send and read messages in the private thread for an exchange to arrange
  pickup.
- Preconditions: The Member is the Poster or the Claimant for that exchange.
- Trigger: The Member opens the exchange thread and posts a message.
- Main success flow:
  1. The Member opens the exchange's message thread.
  2. The Member writes and sends a message.
  3. The system saves the message to the thread.
  4. The system shows the message to both parties.
- Alternate and exception flows:
  - A Member who is not part of the exchange tries to open the thread: the system
    denies access.
  - The message is empty: the system rejects it.
- Postconditions: The message is saved to the exchange thread and visible to both
  parties.
- Related user stories: TBD.

#### Use Case UC-17: View status notifications

- Primary actor: Member
- Supporting actors: System (returns the notifications)
- Goal: Open and read in-app notifications about exchange status changes.
- Preconditions: The Member is logged in.
- Trigger: The Member opens their notifications.
- Main success flow:
  1. The Member opens their notifications.
  2. The system shows the Member's notifications, newest first.
  3. The Member reads a notification and can open the related exchange.
- Alternate and exception flows:
  - There are no notifications: the system shows an empty list.
- Postconditions: The Member has seen their current notifications.
- Related user stories: TBD.

Note: This use case is only the member-initiated viewing of notifications. The
generation of each notification stays as a step or postcondition inside the use
case that caused it (UC-11, UC-12, UC-13, UC-14, and UC-15). The system pushing a
notification is not a use case of its own.

#### Use Case UC-18: Leave a rating and review after completion

- Primary actor: Member (acting as the reviewer)
- Supporting actors: System (saves the review and makes it visible to the
  reviewed party)
- Goal: Leave a short rating and review of the other party after an exchange is
  completed, so the community can build trust.
- Preconditions: The exchange has status COMPLETED. The Member is the Poster or
  the Claimant for that exchange and has not already reviewed the other party.
- Trigger: The Member chooses to leave a review for the completed exchange.
- Main success flow:
  1. The Member opens the completed exchange.
  2. The Member enters a rating and a short review of the other party.
  3. The system saves the review and links it to the completed exchange.
  4. The system makes the saved review visible to the reviewed party
     (requirements.md:82,91).
- Alternate and exception flows:
  - The exchange is not COMPLETED: the system rejects the review and saves
    nothing (requirements.md:93-97).
  - A Member who is not a participant tries to review: the system denies access
    (requirements.md:99-104).
  - The Member has already reviewed the other party for this exchange: the system
    rejects the duplicate review.
- Postconditions: The review is saved, linked to the completed exchange, and
  visible to the reviewed party.
- Related user stories: TBD.

#### Use Case UC-19: View reviews for a completed exchange

- Primary actor: Member
- Supporting actors: System (returns the reviews)
- Goal: View the reviews left for a completed exchange, including a review left
  about the viewing member.
- Preconditions: The exchange has status COMPLETED. The Member is the Poster or
  the Claimant for that exchange.
- Trigger: The Member opens the reviews for a completed exchange.
- Main success flow:
  1. The Member opens a completed exchange.
  2. The system shows the reviews linked to that exchange, including any review
     left about the viewing member (requirements.md:82,91).
- Alternate and exception flows:
  - No review has been left yet: the system shows that there are no reviews yet.
  - A Member who is not a participant tries to view the reviews: the system
    denies access.
- Postconditions: The Member has seen the reviews for the completed exchange.
- Related user stories: TBD.

#### Use Case UC-20: View my dashboard and activity overview

- Primary actor: Member
- Supporting actors: System (gathers the member's activity)
- Goal: See a single overview of the Member's own activity: active listings,
  incoming requests, outgoing requests, and exchange history.
- Preconditions: The Member is logged in.
- Trigger: The Member opens their dashboard.
- Main success flow:
  1. The Member opens their dashboard.
  2. The system gathers the Member's active listings, incoming requests, outgoing
     requests, and exchange history.
  3. The system shows these grouped by status, with each status-based action
     shown as an entry point to its own use case.
- Alternate and exception flows:
  - The Member has no activity yet: the system shows empty groups.
- Postconditions: The Member has seen their current activity overview.
- Related user stories: TBD.

Note: This use case is a single viewing goal. The status-based actions on the
dashboard (deactivate a listing, confirm pickup, cancel a claim, approve or deny
a request, complete an exchange) are surfaced as entry points to their own use
cases (UC-08, UC-12, UC-13, UC-14, and UC-15). They are not performed here. This
keeps the dashboard view separate from the actions, matching the
profile-view versus action-dashboard split in requirements.md:35.

### 3.7 Admin

#### Use Case UC-21: Suspend a user

- Primary actor: Admin
- Supporting actors: System (updates the account)
- Goal: Suspend a member account so it can no longer log in or take member
  actions.
- Preconditions: The Admin is logged in with admin rights. The target account
  exists.
- Trigger: The Admin chooses to suspend a user.
- Main success flow:
  1. The Admin opens the target user account.
  2. The Admin chooses to suspend it.
  3. The system marks the account suspended.
- Alternate and exception flows:
  - A non-admin user tries to suspend a user: the system denies the action.
- Postconditions: The account is suspended and cannot log in or take member
  actions.
- Related user stories: TBD.

#### Use Case UC-22: Deactivate a listing as admin

- Primary actor: Admin
- Supporting actors: System (hides the listing, keeps audit history)
- Goal: Hide a listing from browsing without deleting its audit history.
- Preconditions: The Admin is logged in with admin rights. The listing exists.
- Trigger: The Admin chooses to deactivate a listing.
- Main success flow:
  1. The Admin opens the target listing.
  2. The Admin chooses to deactivate it.
  3. The system hides the listing from browsing and keeps its audit history.
- Alternate and exception flows:
  - A non-admin user tries to deactivate a listing as admin: the system denies
    the action.
- Postconditions: The listing is hidden from browsing and its audit history is
  kept.
- Related user stories: TBD.

#### Use Case UC-23: Generate basic reports

- Primary actor: Admin
- Supporting actors: System (computes and returns the report)
- Goal: Generate a basic report about system activity.
- Preconditions: The Admin is logged in with admin rights.
- Trigger: The Admin chooses to generate a report.
- Main success flow:
  1. The Admin chooses a report to generate.
  2. The system computes the report.
  3. The system shows the report to the Admin.
- Alternate and exception flows:
  - A non-admin user tries to generate a report: the system denies the action.
- Postconditions: The Admin has seen the generated report.
- Related user stories: TBD.

Note: To keep this use case verifiable, assume at least one concrete report
type: an active-listing count and a completed-exchange count. The exact set of
report types is an open question for the cross-team review packet
(use-case-template.md:111,185; requirements.md:332). See Section 6.

### 3.8 Stretch use cases

The next use case is a stretch goal (team-charter.md:42). It is included only if
time allows and is clearly labeled as stretch.

#### Use Case UC-24: View pickup reminders (stretch)

- Primary actor: Member
- Supporting actors: System (generates and returns reminders)
- Goal: View reminders that a listing's pickup window is getting close.
- Preconditions: The Member is logged in and is a party to an exchange with an
  upcoming pickup window.
- Trigger: The Member opens their reminders.
- Main success flow:
  1. The Member opens their reminders.
  2. The system shows reminders for exchanges whose pickup window is getting
     close.
- Alternate and exception flows:
  - There are no upcoming pickups: the system shows an empty list.
- Postconditions: The Member has seen their current pickup reminders.
- Related user stories: TBD.

Note: This is a stretch use case. The generation of each reminder stays a system
step. The member-initiated goal is viewing the reminders, not receiving them.

## 4. Master use-case list

| Use case ID | Name (verb phrase) | Primary actor | Goal (one line) |
| --- | --- | --- | --- |
| UC-01 | Register with an invite token | Guest | Create a member account using a valid invite token |
| UC-02 | Log in | Member | Sign in to reach member features |
| UC-03 | Log out | Member | End the current session |
| UC-04 | Invite a new member | Member | Create and share an invite token (assumption) |
| UC-05 | Manage member profile | Member | View and update profile details |
| UC-06 | Create a listing | Poster | Post a listing with the required details |
| UC-07 | Edit a listing | Poster | Update a listing's details and photos |
| UC-08 | Deactivate own listing | Poster | Take a listing out of browsing |
| UC-09 | Browse, search, and filter active listings | Member | Find active listings of interest |
| UC-10 | View listing details | Member | See one listing's full details |
| UC-11 | Claim produce from a listing | Claimant | Request a set quantity from an active listing |
| UC-12 | Confirm pickup | Claimant | Confirm the item was picked up |
| UC-13 | Cancel a claim | Claimant | Cancel a claim before pickup |
| UC-14 | Approve or deny a claim request | Poster | Decide a pending request on an owned listing |
| UC-15 | Complete an exchange | Poster | Mark a picked-up exchange complete |
| UC-16 | Coordinate pickup via the exchange message thread | Member | Send and read messages for an exchange |
| UC-17 | View status notifications | Member | Open and read in-app notifications |
| UC-18 | Leave a rating and review after completion | Member | Review the other party after completion |
| UC-19 | View reviews for a completed exchange | Member | View reviews linked to a completed exchange |
| UC-20 | View my dashboard and activity overview | Member | See own listings, requests, and history |
| UC-21 | Suspend a user | Admin | Suspend a member account |
| UC-22 | Deactivate a listing as admin | Admin | Hide a listing while keeping audit history |
| UC-23 | Generate basic reports | Admin | Generate a basic activity report |
| UC-24 | View pickup reminders (stretch) | Member | View reminders for upcoming pickups |

## 5. Coverage and traceability

### 5.1 Role-coverage check

Every defined role is the primary actor of at least one use case
(use-case-template.md:190-191).

| Role | Use cases where this role is the primary actor |
| --- | --- |
| Guest | UC-01 |
| Member | UC-02, UC-03, UC-04, UC-05, UC-09, UC-10, UC-16, UC-17, UC-18, UC-19, UC-20, UC-24 |
| Poster | UC-06, UC-07, UC-08, UC-14, UC-15 |
| Claimant | UC-11, UC-12, UC-13 |
| Admin | UC-21, UC-22, UC-23 |

All five roles appear as a primary actor. The role-coverage check passes.

### 5.2 Charter scope to use case

This table traces each charter In-Scope product-behavior item
(team-charter.md:30-43) to the use cases that cover it. Non-use-case charter
items are traced separately in Section 7.

| Charter In-Scope item | Use cases |
| --- | --- |
| Invite-only registration and login, plus member profiles | UC-01, UC-02, UC-03, UC-04, UC-05 |
| Create, edit, and deactivate listings with required details | UC-06, UC-07, UC-08 |
| Photo uploads on listings | UC-06 (optional during create), UC-07 (add, replace, remove) |
| Browse, search, and filter active listings | UC-09, UC-10 |
| Submit and manage claim requests for a specific quantity | UC-11, UC-12, UC-13, UC-14, UC-15 |
| Full claim lifecycle, including over-claim protection | UC-11, UC-12, UC-13, UC-14, UC-15 |
| Private message thread for each exchange | UC-16 |
| In-app notifications for key status changes | UC-11, UC-12, UC-13, UC-14, UC-15 (generate), UC-17 (view) |
| Ratings and reviews after an exchange is completed | UC-18 (leave), UC-19 (view) |
| Member dashboard with status-based actions | UC-20 (view), with actions in UC-08, UC-12, UC-13, UC-14, UC-15 |
| Admin: suspend users, deactivate listings, generate reports | UC-21, UC-22, UC-23 |
| Stretch: pickup reminders | UC-24 |
| Stretch: email or SMS notifications | Dropped from the use-case set; not modeled as a use case |

### 5.3 Requirements to use case

This table traces the key functional requirements, including the claim lifecycle
and the business rules, to the use cases that satisfy them.

| Requirement | Use cases |
| --- | --- |
| Join only via invitation; create a profile | UC-01, UC-04, UC-05 |
| Post listings with required details and optional photos | UC-06, UC-07 |
| Browse and search listings | UC-09, UC-10 |
| Submit a claim for a specified quantity | UC-11 |
| Status REQUESTED -> APPROVED | UC-11 (create REQUESTED), UC-14 (APPROVED) |
| Status APPROVED -> PICKED_UP | UC-12 |
| Status PICKED_UP -> COMPLETED | UC-15 |
| REQUESTED -> DENIED terminal rejection | UC-14 |
| CANCELLED only from REQUESTED or APPROVED | UC-13 |
| No cancellation after pickup | UC-13 (exception flow) |
| Request cannot exceed total quantity available | UC-11 |
| Approved quantities never exceed remaining quantity | UC-14 |
| Reject zero or negative claim quantity | UC-11 |
| Private message thread per exchange | UC-16 |
| Notifications for status changes | UC-11, UC-12, UC-13, UC-14, UC-15, UC-17 |
| Pickup reminders if implemented | UC-24 (stretch) |
| Rating and review after completion; reviewed party can view | UC-18, UC-19 |
| Review not allowed before completion | UC-18 (exception flow) |
| Non-participant cannot review | UC-18 (exception flow) |
| Profile and dashboard of listings, requests, history | UC-20 |
| Admin suspend users | UC-21 |
| Admin deactivate listings, keep audit history | UC-22 |
| Admin generate basic reports | UC-23 |

## 6. Assumptions and open questions

The team has not yet approved the items below. Each must be reviewed and either
confirmed or changed before the requirements are locked. They follow the course
guidance to record open questions and assumptions for the review packet
(requirements.md:332).

| ID | Type | Item | Affected use cases | Status |
| --- | --- | --- | --- | --- |

## 7. Non-use-case charter commitments

The charter commits to several items that are not actor goals, so they are not
use cases (team-charter.md:22-26,39). They are traced here to the later artifacts
that will cover them, so reviewers can confirm nothing was dropped.

| Charter commitment | Covered by (artifact) |
| --- | --- |
| Responsive UI | UI implementation standards and manual or automated UI checks |
| Deployed app, runnable in under 30 minutes | Deployment guide and release-readiness document |
| README | README artifact |
| Seeded demo data showing every major state | Seed data and demo plan |
| Automated tests for permissions and the exchange flow | QA plan and automated test suite |
| At least 70 percent backend business-logic coverage | QA plan and QA summary |
| Pull requests, no direct commits to main, weekly board updates | Project process artifacts, GitHub settings, meeting notes, project board |
