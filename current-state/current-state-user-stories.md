# User Stories: Local Produce Exchange

## 1\. Introduction

This document holds the user stories for the Local Produce Exchange. It is the companion to the team's use cases, the Team Charter, and project requirements. Each story names a user, the goal that user wants, and the benefit they get, followed by acceptance criteria. 

Every story has a "Source use case(s)" so each one is traceable back to the current-scope use cases, which are in turn traceable back to the requirements and the charter. Each story is one distinct functionality. Edge cases and permission rules are captured as scenarios inside the relevant story, not as separate stories, per the discussions in the \#questions Discord channel.

## 2\. The user stories

Scenarios cover the normal path, and add edge or error and permission scenarios only where the action needs them.

### 2.1 Authentication and account

#### US-01: Register with an invite token

- **Source use case:** UC-01 (Register with an invite token)  
- **Priority:** high  
- **Actor:** guest  
- **Milestone:** Current scope (R1)

**Story:** As a guest, I want to register for an account using a valid invite token so that I can join the trusted community.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the guest holds a valid, unused invite token  
- And the guest provides complete account details  
- When the guest submits the registration form with the token  
- Then a member account is created  
- And the invite token is marked as used

*Scenario 2 \- edge / error path (bad token)*

- Given the guest holds a token that is invalid, expired, or already used  
- When the guest submits the registration form with that token  
- Then the system rejects the registration  
- And no account is created

*Scenario 3 \- edge / error path (bad details)*

- Given the guest holds a valid, unused invite token  
- And a required account detail is missing or malformed  
- When the guest submits the registration form  
- Then the system rejects the submission  
- And shows a validation error

**Notes / open questions / assumptions**

- Token expiry rules are not yet defined. Confirm whether tokens expire and after how long.

#### US-02: Log in

- **Source use case:** UC-02 (Log in)  
- **Priority:** high  
- **Actor:** member  
- **Milestone:** Current scope (R1)

**Story:** As a member, I want to log in to my account so that I can reach member features.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the member has a registered account that is not suspended  
- When the member submits correct credentials  
- Then the system starts an authenticated session

*Scenario 2 \- edge / error path (wrong credentials)*

- Given the member has a registered account  
- When the member submits credentials that do not match  
- Then the system denies login  
- And no session is started

*Scenario 3 \- permission rule (suspended account)*

- Given the member's account is suspended  
- When the member submits correct credentials  
- Then the system denies login  
- And tells the member the account is suspended

**Notes / open questions / assumptions**

- Suspending an account is an admin feature held back from this slice, but the suspended state is still checked at login.

#### US-03: Log out

- **Source use case:** UC-03 (Log out)  
- **Priority:** medium  
- **Actor:** member  
- **Milestone:** Current scope (R1)

**Story:** As a member, I want to log out so that no one else can use my account on this device.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the member is logged in  
- When the member chooses to log out  
- Then the system ends the session  
- And returns the member to the public view

*Scenario 2 \- edge / error path (session already expired)*

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
- **Milestone:** Current scope (R1)

**Story:** As a member, I want to create and share an invite token so that a new person can register and join the community.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the member is logged in and active  
- And the member is allowed to issue invites  
- When the member chooses to create an invite  
- Then the system generates a new, unused invite token  
- And shows the token so the member can share it

*Scenario 2 \- permission rule (not allowed to invite)*

- Given the member is not allowed to issue invites  
- When the member attempts to create an invite  
- Then the system denies the action  
- And no token is created

*Scenario 3 \- permission rule (suspended member)*

- Given the member is suspended  
- When the member attempts to create an invite  
- Then the system denies the action  
- And no token is created

**Notes / open questions / assumptions**

- ASSUMPTION: the source documents require invite-only registration but do not say who issues invite tokens. Confirm whether any active member can invite, or only an admin. If the team does not decide the issuer, keep US-01 as core and treat US-04 as optional or pending.

#### US-05: View and update member profile

- **Source use case:** UC-05 (Manage member profile)  
- **Priority:** medium  
- **Actor:** member  
- **Milestone:** Current scope (R1)

**Story:** As a member, I want to view and update my profile details so that my information stays current.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the member is logged in  
- When the member edits a profile field and saves  
- Then the system validates the change  
- And saves the updated profile

*Scenario 2 \- edge / error path (bad field)*

- Given the member is logged in  
- And a profile field is missing or malformed  
- When the member saves the change  
- Then the system rejects the change  
- And shows a validation error

*Scenario 3 \- permission rule (editing another profile)*

- Given the member is logged in  
- When the member attempts to edit another member's profile  
- Then the system denies the action  
- And nothing changes

**Notes / open questions / assumptions**

- None.

### 2.2 Discovery

#### US-06: Browse, search, and filter active listings

- **Source use case:** UC-06 (Browse, search, and filter active listings)  
- **Priority:** high  
- **Actor:** member  
- **Milestone:** Current scope (R1)

**Story:** As a member, I want to browse, search, and filter active listings so that I can find items of interest.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the member is logged in  
- When the member enters search terms or chooses filters such as category or dietary and allergen tags  
- Then the system returns the active listings that match

*Scenario 2 \- edge / error path (no matches)*

- Given the member is logged in  
- When no active listings match the search or filters  
- Then the system shows an empty result

**Notes / open questions / assumptions**

- Active listings are seeded for this slice; creating listings is out of the current scope.  
- Confirm the exact filter set. At minimum: category and dietary and allergen tags.

#### US-07: View listing details

- **Source use case:** UC-07 (View listing details)  
- **Priority:** high  
- **Actor:** member  
- **Milestone:** Current scope (R1)

**Story:** As a member, I want to see the full details of one listing so that I can decide whether to request it.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the member is logged in  
- And the listing is active  
- When the member selects the listing  
- Then the system shows its full details, including the quantity available, the tags, and the pickup window

*Scenario 2 \- edge / error path (no longer active)*

- Given the member selects a listing that is no longer active  
- When the system loads the listing  
- Then the system tells the member the listing is unavailable

**Notes / open questions / assumptions**

- Photos are not part of the current listing detail view in this slice.

### 2.3 Request queue

#### US-08: Submit a request for an item

- **Source use case:** UC-08 (Submit a request for an item)  
- **Priority:** high  
- **Actor:** recipient  
- **Milestone:** Current scope (R1)

**Story:** As a recipient, I want to submit a request for a specified quantity from a listing so that my request joins the listing's queue and is handled in order.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given a listing is active  
- And the listing has 10 units available  
- When the recipient requests 3 units  
- Then a request is created with status REQUESTED  
- And the request is placed at the end of the listing's queue  
- And the poster is notified of the new request

*Scenario 2 \- edge / error path (quantity exceeds available)*

- Given a listing is active  
- And the listing has 10 units available  
- When the recipient requests 12 units  
- Then the system rejects the request  
- And no request is created

*Scenario 3 \- edge / error path (quantity must be positive)*

- Given a listing is active  
- When the recipient requests 0 units  
- Then the system rejects the request  
- And shows a validation error

*Scenario 4 \- edge / error path (duplicate open request)*

- Given the recipient already has an open request on this listing  
- When the recipient attempts to submit another request on the same listing  
- Then the system prevents the duplicate request

*Scenario 5 \- permission rule (suspended member)*

- Given the user submitting the request is suspended  
- When the suspended user attempts to submit a request  
- Then the system denies access  
- And no request is created

**Notes / open questions / assumptions**

- This story models the requirements Sample User Story \#1, adapted to the request-queue scope.

#### US-09: View the request queue for a listing

- **Source use case:** UC-09 (View the request queue for a listing)  
- **Priority:** high  
- **Actor:** poster  
- **Milestone:** Current scope (R1)

**Story:** As a poster, I want to see the pending requests on my listing in the order they were received so that I can handle them fairly and in order.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the poster owns the listing  
- When the poster opens the listing's request queue  
- Then the system shows the pending requests oldest first, with the quantity requested and the quantity remaining

*Scenario 2 \- edge / error path (empty queue)*

- Given the poster owns a listing with no pending requests  
- When the poster opens the request queue  
- Then the system shows an empty queue

*Scenario 3 \- permission rule (not the owner)*

- Given a member who does not own the listing  
- When that member attempts to view the listing's queue  
- Then the system denies access

**Notes / open questions / assumptions**

- None.

#### US-10: Approve or deny the next request in the queue

- **Source use case:** UC-10 (Approve or deny the next request in the queue)  
- **Priority:** high  
- **Actor:** poster  
- **Milestone:** Current scope (R1)

**Story:** As a poster, I want to approve or deny a pending request on my listing so that I control who receives my items, in order, without conflicts.

**Acceptance criteria**

*Scenario 1 \- normal / valid path (approve)*

- Given the poster owns the listing  
- And the request has status REQUESTED  
- And the approved quantity will not exceed the remaining quantity  
- When the poster approves the request  
- Then the system sets the request status to APPROVED  
- And reduces the remaining quantity  
- And the recipient is notified

*Scenario 2 \- normal / valid path (deny)*

- Given the poster owns the listing  
- And the request has status REQUESTED  
- When the poster denies the request  
- Then the system sets the request status to DENIED  
- And the recipient is notified

*Scenario 3 \- workflow rule (conflict prevention)*

- Given the poster owns the listing  
- And approving this request would push the approved quantity past the remaining quantity  
- When the poster attempts to approve it  
- Then the system rejects the approval  
- And the status and remaining quantity do not change

*Scenario 4 \- workflow rule (wrong status)*

- Given the request is not in REQUESTED status  
- When the poster attempts to approve or deny it  
- Then the system rejects the action  
- And nothing changes

*Scenario 5 \- permission rule (not the listing owner)*

- Given a member who does not own the listing  
- When that member attempts to approve or deny the request  
- Then the system denies access  
- And nothing changes

**Notes / open questions / assumptions**

- DENIED is terminal.  
- ASSUMPTION: the poster handles requests in the order received. Confirm this with the team.  
- Approve and deny are two outcomes of one decision, so they are scenarios in this one story, not separate stories.

#### US-11: Withdraw a queued request

- **Source use case:** UC-11 (Withdraw a queued request)  
- **Priority:** medium  
- **Actor:** recipient  
- **Milestone:** Current scope (R1)

**Story:** As a recipient, I want to withdraw a pending request I no longer need so that it leaves the queue and the poster knows.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the recipient owns a request with status REQUESTED  
- When the recipient withdraws it  
- Then the system sets the request status to CANCELLED  
- And removes it from the queue  
- And the poster is notified

*Scenario 2 \- workflow rule (already handled)*

- Given the request has status APPROVED or DENIED  
- When the recipient attempts to withdraw it  
- Then the system rejects the withdrawal  
- And nothing changes

*Scenario 3 \- permission rule (not the requester)*

- Given a member who is not the requester for the request  
- When that member attempts to withdraw it  
- Then the system denies the action  
- And nothing changes

**Notes / open questions / assumptions**

- CANCELLED is terminal. In this scope, only a pending request can be withdrawn.

### 2.4 Coordination

#### US-12: Send and read messages in the exchange thread

- **Source use case:** UC-12 (Send and read messages in the exchange thread)  
- **Priority:** medium  
- **Actor:** member  
- **Milestone:** Current scope (R1)

**Story:** As a member who is the poster or the recipient for an exchange, I want to send and read messages in that exchange's private thread so that the two of us can coordinate the exchange.

**Acceptance criteria**

*Scenario 1 \- normal / valid path*

- Given the member is the poster or the recipient for the exchange  
- When the member writes and sends a message  
- Then the system saves the message to the thread  
- And shows it to both parties

*Scenario 2 \- edge / error path (empty message)*

- Given the member is a party to the exchange  
- When the member sends an empty message  
- Then the system rejects it

*Scenario 3 \- permission rule (not a party)*

- Given a member who is not the poster or the recipient for the exchange  
- When that member attempts to open the thread  
- Then the system denies access

**Notes / open questions / assumptions**

- One thread links a poster and a recipient for a request.

## 3\. Traceability table

This table is the authoritative story list for the current scope. It traces each story back to its source use case and forward to the requirement or in-scope deliverable it satisfies. This gives two-way traceability: requirements and in-scope deliverables \-\> current-scope use cases \-\> user stories.

| Story ID | Title | Source use case | Requirement / in-scope source |
| :---- | :---- | :---- | :---- |
| US-01 | Register with an invite token | UC-01 | Join only via invitation; create a member account |
| US-02 | Log in | UC-02 | Member login |
| US-03 | Log out | UC-03 | Member logout |
| US-04 | Invite a new member | UC-04 | Invite-only access; issue invite tokens (assumption) |
| US-05 | View and update member profile | UC-05 | Member profiles |
| US-06 | Browse, search, and filter active listings | UC-06 | Browse, search, and filter active listings |
| US-07 | View listing details | UC-07 | View one listing's full details |
| US-08 | Submit a request for an item | UC-08 | Submit a request for a specified quantity |
| US-09 | View the request queue for a listing | UC-09 | Requests handled in the order received |
| US-10 | Approve or deny the next request in the queue | UC-10 | Handle requests in order; no over-claim |
| US-11 | Withdraw a queued request | UC-11 | Withdraw a pending request |
| US-12 | Send and read messages in the exchange thread | UC-12 | Private message thread per exchange |

## 4\. Assumptions and open questions

These items are not yet approved by the team. Each one should be confirmed or changed before the requirements are locked. They are also noted inline in the affected stories.

| ID | Item | Affected stories | Status |
| :---- | :---- | :---- | :---- |
|  |  |  |  |

