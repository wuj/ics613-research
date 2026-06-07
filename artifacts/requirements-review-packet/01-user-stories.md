# User Stories

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu

These are the team's user stories. Each story's acceptance criteria are in the acceptance criteria document under the same US-ID.

### Authentication and account

#### US-01: Register with an invite token

**Story:** As a guest, I want to register for an account using a valid invite token so that I can join the trusted community.

- **Source use case:** UC-01 (Register with an invite token)
- **Priority:** high
- **Actor:** guest
- **Milestone:** Current scope (R1)

#### US-02: Log in

**Story:** As a member, I want to log in to my account so that I can reach member features.

- **Source use case:** UC-02 (Log in)
- **Priority:** high
- **Actor:** member
- **Milestone:** Current scope (R1)

#### US-03: Log out

**Story:** As a member, I want to log out so that no one else can use my account on this device.

- **Source use case:** UC-03 (Log out)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Current scope (R1)

#### US-04: Invite a new member

**Story:** As a member, I want to create and share an invite token so that a new person can register and join the community.

- **Source use case:** UC-04 (Invite a new member)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Current scope (R1)

#### US-05: View and update member profile

**Story:** As a member, I want to view and update my profile details so that my information stays current.

- **Source use case:** UC-05 (Manage member profile)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Current scope (R1)

### Discovery

#### US-06: Browse, search, and filter active listings

**Story:** As a member, I want to browse, search, and filter active listings so that I can find items I want.

- **Source use case:** UC-06 (Browse, search, and filter active listings)
- **Priority:** high
- **Actor:** member
- **Milestone:** Current scope (R1)

#### US-07: View listing details

**Story:** As a member, I want to see the full details of one listing so that I can decide whether to request it.

- **Source use case:** UC-07 (View listing details)
- **Priority:** high
- **Actor:** member
- **Milestone:** Current scope (R1)

#### US-30: View another member's public profile

**Story:** As a member, I want to view another member's public profile and review history so that I can assess trustworthiness before requesting or approving an exchange

- **Source use case:** UC-26 (View another member's public profile)
- **Priority:** low
- **Actor:** member
- **Milestone:** Full Features (R2)

### Request queue

#### US-08: Submit a request for an item

**Story:** As a recipient, I want to submit a request for a specified quantity from a listing so that my request joins the listing's queue and is handled in order.

- **Source use case:** UC-08 (Submit a request for an item)
- **Priority:** high
- **Actor:** recipient
- **Milestone:** Current scope (R1)

#### US-09: View the request queue for a listing

**Story:** As a poster, I want to see the pending requests on my listing in the order they were received so that I can handle them fairly and in order.

- **Source use case:** UC-09 (View the request queue for a listing)
- **Priority:** high
- **Actor:** poster
- **Milestone:** Current scope (R1)

#### US-10: Approve or deny a pending request

**Story:** As a poster, I want to approve or deny a pending request on my listing so that I control who receives my items, in order, without conflicts.

- **Source use case:** UC-10 (Approve or deny the next request in the queue)
- **Priority:** high
- **Actor:** poster
- **Milestone:** Current scope (R1)

#### US-11: Withdraw a queued request

**Story:** As a recipient, I want to withdraw a pending request I no longer need so that it leaves the queue and the poster knows.

- **Source use case:** UC-11 (Withdraw a queued request)
- **Priority:** medium
- **Actor:** recipient
- **Milestone:** Current scope (R1)

#### US-26: Cancel an approved request

**Story:** As a recipient, I want to cancel a request that has been approved but not yet picked up, so that I can free up quantity for others if I no longer need the item

- **Source use case:** UC-11 (Withdraw a queued request)
- **Priority:** medium
- **Actor:** recipient
- **Milestone:** Follow up scope (R2)

### Coordination

#### US-12: Send and read messages in the exchange thread

**Story:** As a member who is the poster or the recipient for an exchange, I want to send and read messages in that exchange's private thread so that the two of us can coordinate the exchange.

- **Source use case:** UC-12 (Send and read messages in the exchange thread)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Current scope (R1)

### Listings

#### US-13: Create a listing

**Story:** As a poster, I want to post a new listing with the required details so that other members can find and request the item.

- **Source use case:** UC-13 (Create a listing)
- **Priority:** high
- **Actor:** poster
- **Milestone:** Full features (R2)

#### US-14: Edit a listing

**Story:** As a poster, I want to update the details of one of my listings so that the information stays accurate.

- **Source use case:** UC-14 (Edit a listing)
- **Priority:** medium
- **Actor:** poster
- **Milestone:** Full features (R2)

#### US-15: Deactivate own listing

**Story:** As a poster, I want to deactivate one of my listings so that no new requests can be made on it.

- **Source use case:** UC-15 (Deactivate own listing)
- **Priority:** medium
- **Actor:** poster
- **Milestone:** Full features (R2)

### Pickup and completion

#### US-16: Confirm pickup

**Story:** As a recipient, I want to confirm that I picked up the item so that the exchange can be completed.

- **Source use case:** UC-16 (Confirm pickup)
- **Priority:** high
- **Actor:** recipient
- **Milestone:** Full features (R2)

#### US-17: Complete an exchange

**Story:** As a poster, I want to mark a picked-up exchange as completed so that both parties can leave a review.

- **Source use case:** UC-17 (Complete an exchange)
- **Priority:** high
- **Actor:** poster
- **Milestone:** Full features (R2)

### Reviews

#### US-18: Leave a rating and review after completion

**Story:** As a member who took part in a completed exchange, I want to leave a short rating and review of the other party so that the two of us can build trust.

- **Source use case:** UC-18 (Leave a rating and review after completion)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Full features (R2)

#### US-19: View reviews for a completed exchange

**Story:** As a member, I want to view the reviews left for a completed exchange so that I can see feedback, including any review left about me.

- **Source use case:** UC-19 (View reviews for a completed exchange)
- **Priority:** low
- **Actor:** member
- **Milestone:** Full features (R2)

### Notifications and dashboard

#### US-20: View status notifications

**Story:** As a member, I want to open and read in-app notifications about exchange status changes so that I know when I need to act.

- **Source use case:** UC-20 (View status notifications)
- **Priority:** medium
- **Actor:** member
- **Milestone:** Full features (R2)

#### US-28: Mark a notification as read

**Story:** As a member, I want to mark a notification as read so that I can track which status changes I have already seen.

- **Source use case:** UC-20
- **Priority:** low
- **Actor:** member
- **Milestone:** Full features (R2)

#### US-21: View my dashboard and activity overview

**Story:** As a member, I want a single dashboard of my activity so that I can see my active listings, incoming and outgoing requests, and exchange history in one place.

- **Source use case:** UC-21 (View my dashboard and activity overview)
- **Priority:** high
- **Actor:** member
- **Milestone:** Full features (R2)

### Admin

#### US-22: Suspend a user

**Story:** As an admin, I want to suspend a member account so that a misbehaving user can no longer log in or take member actions.

- **Source use case:** UC-22 (Suspend a user)
- **Priority:** medium
- **Actor:** admin
- **Milestone:** Full features (R2)

#### US-27: Unsuspend a user

**Story:** As an admin, I want to unsuspend a member account so that a user who was incorrectly flagged or has taken steps to remediate behavior can have their account reinstated

- **Source use case:** UC-22 (Suspend a user)
- **Priority:** medium
- **Actor:** admin
- **Milestone:** Full features (R2)

#### US-23: Deactivate a listing as admin

**Story:** As an admin, I want to deactivate any listing so that I can hide it from browsing without deleting its audit history.

- **Source use case:** UC-23 (Deactivate a listing as admin)
- **Priority:** medium
- **Actor:** admin
- **Milestone:** Full features (R2)

#### US-24: Generate basic reports

**Story:** As an admin, I want to generate a basic report about system activity so that I can see how the exchange is being used.

- **Source use case:** UC-24 (Generate basic reports)
- **Priority:** low
- **Actor:** admin
- **Milestone:** Full features (R2)

#### US-29: View member profile as admin

**Story:** As an admin, I want to search and view a member's full account details so that I can find the right account before taking administrative action.

- **Source use case:** UC-25 (View member profile as admin)
- **Priority:** medium
- **Actor:** admin
- **Milestone:** Full features (R2)

### Listing photos

#### US-25: Add and manage listing photos

**Story:** As a poster, I want to add, replace, and remove photos on my listing so that members can see what the item looks like.

- **Source use case:** UC-13 (optional photos during create), UC-14 (add, replace, or remove photos during edit)
- **Priority:** low
- **Actor:** poster
- **Milestone:** Full features (R2)
