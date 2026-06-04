# Use Cases: Local Produce Exchange

## 1\. Introduction

This document is the high-level use cases for the Local Produce Exchange. It is based on the in-scope items from the Team Charter and project requirements. Each use case follows the recommended format and stays at a high level. Deeper detail is left to the user stories and their acceptance criteria, which are a different deliverable.

The roles used here are Guest, Member, Poster, and Recipient.

## 2\. Actors and Roles

These are the roles the current scope defines. Poster and Recipient are not separate accounts. They are the role a Member plays for a given request. The same person can be a Poster on one listing and a Recipient on another.

* Guest: a person who holds an invite token but has not registered yet. A Guest can only register.  
* Member: a registered, active user. A Member can browse, search, and filter listings, manage a profile, submit requests, and send messages.  
* Poster: the Member who owns a listing. A Poster handles that listing's request queue and messages the recipient. In this scope slice, active listings are provided through seeded data; creating listings is out of the current scope.  
* Recipient: the Member who submits a request for an item from a listing and, if approved, receives it. The poster reaches the recipient through the message thread.

## 3\. Use Cases

The use cases below are grouped by topic.

### 3.1 Authentication and account

#### Use Case UC-01: Register with an invite token

- Primary actor: Guest  
- Supporting actors: System (validates the token)  
- Goal: Create a member account using a valid invite token so the Guest can join the community.  
- Preconditions: The Guest holds an invite token. The token is valid and has not been used.  
- Trigger: The Guest opens the registration page and submits account details with the token.  
- Main success flow:  
  1. The Guest opens the registration page.  
  2. The Guest enters account details and the invite token.  
  3. The system checks that the token is valid and unused.  
  4. The system creates the member account.  
  5. The system marks the token as used.  
- Alternate and exception flows:  
  - Token is invalid, expired, or already used: the system rejects registration and creates no account.  
  - Required account details are missing or malformed: the system rejects the submission and shows a validation error.  
- Postconditions: A member account exists and the invite token is marked as used.  
- Related user stories: TBD.

#### Use Case UC-02: Log in

- Primary actor: Member  
- Supporting actors: System (checks credentials)  
- Goal: Sign in to a member account to reach member features.  
- Preconditions: The Member has a registered account that is not suspended.  
- Trigger: The Member submits login credentials.  
- Main success flow:  
  - The Member opens the login page.  
  - The Member enters credentials.  
  - The system checks the credentials.  
  - The system starts an authenticated session.  
- Alternate and exception flows:  
  - Credentials do not match: the system denies login and starts no session.  
  - The account is suspended: the system denies login and tells the Member the account is suspended.  
- Postconditions: The Member has an active authenticated session.  
- Related user stories: TBD.

#### Use Case UC-03: Log out

- Primary actor: Member  
- Supporting actors: System (ends the session)  
- Goal: End the current session so no one else can use the account on this device.  
- Preconditions: The Member is logged in.  
- Trigger: The Member chooses to log out.  
- Main success flow:  
  1. The Member chooses to log out.  
  2. The system ends the authenticated session.  
  3. The system returns the Member to the public view.  
- Alternate and exception flows:  
  - The session has already expired: the system simply returns the Member to the public view.  
- Postconditions: The Member has no active session.  
- Related user stories: TBD.

#### Use Case UC-04: Invite a new member

- Primary actor: Member  
- Supporting actors: System (creates the invite token)  
- Goal: Create and share an invite token so a new person can register.  
- Preconditions: The Member is logged in and active. (Assumption: a Member may issue invite tokens.)  
- Trigger: The Member chooses to invite a new person.  
- Main success flow:  
  1. The Member chooses to create an invite.  
  2. The system generates a new, unused invite token.  
  3. The system shows the token so the Member can share it.  
- Alternate and exception flows:  
  - The issuer is not allowed to invite: the system denies the action and creates no token.  
  - The member is suspended: the system denies the action and creates no token.  
- Postconditions: A new, unused invite token exists.  
- Related user stories: TBD.

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
  - A profile field is missing or malformed: the system rejects the change and shows a validation error.  
  - A Member tries to edit another member's profile: the system denies the action.  
- Postconditions: The Member's profile reflects the saved changes.  
- Related user stories: TBD.

### 3.2 Discovery

#### Use Case UC-06: Browse, search, and filter active listings

- Primary actor: Member  
- Supporting actors: System (returns matching listings)  
- Goal: Find active listings of interest by browsing, searching, and filtering.  
- Preconditions: The Member is logged in.  
- Trigger: The Member opens the listings view or enters search and filter terms.  
- Main success flow:  
  - The Member opens the listings view.  
  - The Member enters search terms or chooses filters such as category or dietary and allergen tags.  
  - The system returns the active listings that match.  
- Alternate and exception flows:  
  - No listings match: the system shows an empty result.  
- Postconditions: The Member sees the set of active listings that match.  
- Related user stories: TBD.

#### Use Case UC-07: View listing details

- Primary actor: Member  
- Supporting actors: System (returns the listing)  
- Goal: See the full details of one listing before deciding to request it.  
- Preconditions: The Member is logged in and the listing is active.  
- Trigger: The Member selects a listing.  
- Main success flow:  
  1. The Member selects a listing.  
  2. The system shows the listing's full details, including the quantity available, the tags, and the pickup window.  
- Alternate and exception flows:  
  - The listing is no longer active: the system tells the Member the listing is unavailable.  
- Postconditions: The Member has seen the listing's full details.  
- Related user stories: TBD.

### 3.3 Request queue

#### Use Case UC-08: Submit a request for an item

- Primary actor: Recipient  
- Supporting actors: Poster (owns the listing); System (checks quantity, queues the request, notifies the Poster)  
- Goal: Request a specific quantity from an active listing so the request enters the listing's queue and is handled in order.  
- Preconditions: The Recipient is a registered, active member. The listing is active and has a quantity available.  
- Trigger: The Recipient opens an active listing and chooses to request it.  
- Main success flow:  
  1. The Recipient opens an active listing.  
  2. The Recipient enters the quantity they want.  
  3. The system checks the amount against the total quantity available.  
  4. The system creates a request with status REQUESTED and places it at the end of the listing's queue.  
  5. The system notifies the Poster of the new request.  
- Alternate and exception flows:  
  - Requested amount is more than the total quantity available: the system rejects the request and creates no request.  
  - Requested amount is zero or negative: the system rejects it and shows a validation error.  
  - The Recipient already has an open request on this listing: the system prevents a duplicate request.  
- Postconditions: A request exists with status REQUESTED, queued in the order it was received, and the Poster has been notified.  
- Related user stories: TBD.

#### Use Case UC-09: View the request queue for a listing

- Primary actor: Poster  
- Supporting actors: System (returns the queue)  
- Goal: See the pending requests on the Poster's listing in the order they were received.  
- Preconditions: The Poster owns the listing.  
- Trigger: The Poster opens the request queue for one of their listings.  
- Main success flow:  
  1. The Poster opens one of their listings.  
  2. The system shows the pending requests in the order received, oldest first, with the quantity requested and the quantity remaining.  
- Alternate and exception flows:  
  - A Member who does not own the listing tries to view its queue: the system denies access.  
  - There are no pending requests: the system shows an empty queue.  
- Postconditions: The Poster has seen the current request queue.  
- Related user stories: TBD.

#### Use Case UC-10: Approve or deny the next request in the queue

- Primary actor: Poster  
- Supporting actors: Recipient (the requester); System (checks quantity, updates status, notifies the Recipient)  
- Goal: Handle a pending request on the Poster's listing by approving or denying it, keeping the queue order and preventing conflicts.  
- Preconditions: The Poster owns the listing. At least one request has status REQUESTED. (Assumption: the Poster handles requests in the order received.)  
- Trigger: The Poster chooses to approve or deny a pending request.  
- Main success flow:  
  1. The Poster opens the next pending request in the queue.  
  2. The Poster chooses to approve or deny it.  
  3. If approving, the system checks that the approved quantity does not exceed the remaining quantity.  
  4. The system sets the request status to APPROVED or DENIED and, when approving, reduces the remaining quantity.  
  5. The system notifies the Recipient.  
- Alternate and exception flows:  
  - Approving would push the approved quantity past the remaining quantity: the system rejects the approval, which is the conflict the queue prevents.  
  - A Member who does not own the listing tries to handle the request: the system denies access.  
  - The request is not in REQUESTED status: the system rejects the action.  
- Postconditions: The request has status APPROVED or DENIED, the remaining quantity reflects any approval, and the Recipient has been notified. DENIED is terminal.  
- Related user stories: TBD.

#### Use Case UC-11: Withdraw a queued request

- Primary actor: Recipient  
- Supporting actors: Poster (the other party); System (updates status, removes the request from the queue, notifies the Poster)  
- Goal: Withdraw a pending request the Recipient no longer needs, removing it from the queue.  
- Preconditions: The request has status REQUESTED and the Recipient owns it.  
- Trigger: The Recipient chooses to withdraw their request.  
- Main success flow:  
  1. The Recipient opens their pending request.  
  2. The Recipient chooses to withdraw it.  
  3. The system sets the request status to CANCELLED and removes it from the queue.  
  4. The system notifies the Poster.  
- Alternate and exception flows:  
  - The request is already APPROVED or DENIED: the system rejects the withdrawal, because only a pending request can be withdrawn in this scope.  
  - A Member who is not the requester tries to withdraw: the system denies the action.  
- Postconditions: The request has status CANCELLED, which is terminal, it is removed from the queue, and the Poster has been notified.  
- Related user stories: TBD.

### 3.4 Coordination

#### Use Case UC-12: Send and read messages in the exchange thread

- Primary actor: Member (the Poster or the Recipient for the exchange)  
- Supporting actors: System (stores and delivers the message)  
- Goal: Send and read messages in the private thread that links a Poster and a Recipient for a request, so the two can coordinate the exchange.  
- Preconditions: A request connects the Member as the Poster or the Recipient for that listing.  
- Trigger: The Member opens the exchange thread and posts a message.  
- Main success flow:  
  - The Member opens the exchange thread for the request.  
  - The Member writes and sends a message.  
  - The system saves the message to the thread.  
  - The system shows the message to both parties.  
- Alternate and exception flows:  
  - A Member who is not the Poster or the Recipient for that exchange tries to open the thread: the system denies access.  
  - The message is empty: the system rejects it.  
- Postconditions: The message is saved to the exchange thread and visible to both parties.  
- Related user stories: TBD.

## 4\. Assumptions and open questions

The team has not yet approved the items below. Each must be reviewed and either confirmed or changed before the requirements are locked. They follow the course guidance to record open questions and assumptions for the review packet.

| ID | Type | Item | Affected use cases | Status |
| :---- | :---- | :---- | :---- | :---- |
|  |  |  |  |  |

