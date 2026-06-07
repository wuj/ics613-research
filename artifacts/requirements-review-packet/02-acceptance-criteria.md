# Acceptance Criteria

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu

These are the acceptance criteria for every user story, keyed by US-ID. The story statements and metadata are in the user stories document.

### Authentication and account

#### US-01: Register with an invite token

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

#### US-02: Log in

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

#### US-03: Log out

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

#### US-04: Invite a new member

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

#### US-05: View and update member profile

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

### Discovery

#### US-06: Browse, search, and filter active listings

*Scenario 1 - normal / valid path*

- Given the member is logged in
- When the member enters search terms or chooses filters such as category or dietary and allergen tags
- Then the system returns the active listings that match

*Scenario 2 - edge / error path (no matches)*

- Given the member is logged in
- When no active listings match the search or filters
- Then the system shows an empty result

#### US-07: View listing details

*Scenario 1 - normal / valid path*

- Given the member is logged in
- And the listing is active
- When the member selects the listing
- Then the system shows its full details, including the quantity available, the tags, and the pickup window

*Scenario 2 - edge / error path (no longer active)*

- Given the member selects a listing that is no longer active
- When the system loads the listing
- Then the system tells the member the listing is unavailable

**Notes**

- Photos are not part of the current listing detail view.

#### US-30: View another member's public profile

*Scenario 1 - normal / valid path*

- Given the member is logged in
- When the member opens another member's public profile
- Then the system shows that member's display name and their review history

*Scenario 2 - edge / error path (own profile)*

- Given the member opens their own profile through this view
- When the system loads the profile
- Then the system shows their own public profile the same way another member would see it.

*Scenario 3 - permission rule (not logged in)*

- Given a guest who is not logged in attempts to view a member profile
- When the system receives the request
- Then the system denies access

**Notes**

- Private exchange details are not shown in this view.

### Request queue

#### US-08: Submit a request for an item

*Scenario 1 - normal / valid path*

- Given a listing is active
- And the listing has 10 units available
- When the recipient requests 3 units
- Then a request is created with status REQUESTED
- And the request is placed at the end of the listing's queue
- And the poster is notified of the new request

*Scenario 2 - edge / error path (quantity exceeds available)*

- Given a listing is active
- And the listing has 10 units available
- When the recipient requests 12 units
- Then the system rejects the request
- And no request is created

*Scenario 3 - edge / error path (quantity must be positive)*

- Given a listing is active
- When the recipient requests 0 units
- Then the system rejects the request
- And shows a validation error

*Scenario 4 - edge / error path (duplicate open request)*

- Given the recipient already has an open request on this listing
- When the recipient attempts to submit another request on the same listing
- Then the system prevents the duplicate request

*Scenario 5 - permission rule (suspended member)*

- Given the user submitting the request is suspended
- When the suspended user attempts to submit a request
- Then the system denies access
- And no request is created

#### US-09: View the request queue for a listing

*Scenario 1 - normal / valid path*

- Given the poster owns the listing
- When the poster opens the listing's request queue
- Then the system shows the pending requests oldest first, with the quantity requested and the quantity remaining

*Scenario 2 - edge / error path (empty queue)*

- Given the poster owns a listing with no pending requests
- When the poster opens the request queue
- Then the system shows an empty queue

*Scenario 3 - permission rule (not the owner)*

- Given a member who does not own the listing
- When that member attempts to view the listing's queue
- Then the system denies access

#### US-10: Approve or deny a pending request

*Scenario 1 - normal / valid path (approve)*

- Given the poster owns the listing
- And the request has status REQUESTED
- And the approved quantity will not exceed the remaining quantity
- When the poster approves the request
- Then the system sets the request status to APPROVED
- And reduces the remaining quantity
- And the recipient is notified

*Scenario 2 - normal / valid path (deny)*

- Given the poster owns the listing
- And the request has status REQUESTED
- When the poster denies the request
- Then the system sets the request status to DENIED
- And the recipient is notified

*Scenario 3 - workflow rule (conflict prevention)*

- Given the poster owns the listing
- And approving this request would push the approved quantity past the remaining quantity
- When the poster attempts to approve it
- Then the system rejects the approval
- And the status and remaining quantity do not change

*Scenario 4 - workflow rule (wrong status)*

- Given the request is not in REQUESTED status
- When the poster attempts to approve or deny it
- Then the system rejects the action
- And nothing changes

*Scenario 5 - permission rule (not the listing owner)*

- Given a member who does not own the listing
- When that member attempts to approve or deny the request
- Then the system denies access
- And nothing changes

**Notes**

- A denial is final: the request cannot change status again.

#### US-11: Withdraw a queued request

*Scenario 1 - normal / valid path*

- Given the recipient owns a request with status REQUESTED
- When the recipient withdraws it
- Then the system sets the request status to CANCELLED
- And removes it from the queue
- And the poster is notified

*Scenario 2 - workflow rule (not pending)*

- Given the request is not in REQUESTED status
- When the recipient attempts to withdraw it
- Then the system rejects the withdrawal
- And nothing changes

*Scenario 3 - permission rule (not the requester)*

- Given a member who is not the requester for the request
- When that member attempts to withdraw it
- Then the system denies the action
- And nothing changes

**Notes**

- Only a pending request can be withdrawn.

#### US-26: Cancel an approved request

*Scenario 1 - normal / valid path*

- Given the recipient owns a request with status APPROVED
- When the recipient cancels it
- The system sets the request to CANCELLED
- Restaurants the quantity back to the listing's quantity
- And the poster is notified

*Scenario 2 - workflow rule (wrong status)*

- Given the request is in terminal status PICKED_UP, DENIED, CANCELLED
- When the recipient attempts to cancel it
- The system rejects the action
- Nothing changes

*Scenario 3 - permission rule (not the requester)*

- Given a member who is not the requester for the request
- When that member attempts to cancel it
- The system denies the action
- Nothing changes

### Coordination

#### US-12: Send and read messages in the exchange thread

*Scenario 1 - normal / valid path*

- Given the member is the poster or the recipient for the exchange
- When the member writes and sends a message
- Then the system saves the message to the thread
- And shows it to both parties

*Scenario 2 - edge / error path (empty message)*

- Given the member is a party to the exchange
- When the member sends an empty message
- Then the system rejects it

*Scenario 3 - permission rule (not a party)*

- Given a member who is not the poster or the recipient for the exchange
- When that member attempts to open the thread
- Then the system denies access

**Notes**

- One thread links a poster and a recipient for a request.

### Listings

#### US-13: Create a listing

*Scenario 1 - normal / valid path*

- Given the poster is a registered, active member
- When the poster enters a description, category, quantity available, dietary and allergen tags, and a pickup window, and saves
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

**Notes**

- Optional photo uploads during create are covered by US-25.
- The listing's remaining quantity starts equal to the quantity available.

#### US-14: Edit a listing

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

**Notes**

- Adding, replacing, or removing photos during edit is covered by US-25.

#### US-15: Deactivate own listing

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

### Pickup and completion

#### US-16: Confirm pickup

*Scenario 1 - normal / valid path*

- Given the recipient owns a request with status APPROVED
- When the recipient confirms pickup
- Then the system changes the request status from APPROVED to PICKED_UP
- And the poster is notified

*Scenario 2 - workflow rule (wrong status)*

- Given the request is not in APPROVED status
- When the recipient attempts to confirm pickup
- Then the system rejects the action
- And nothing changes

*Scenario 3 - permission rule (not the requester)*

- Given a member who is not the requester for the request
- When that member attempts to confirm pickup
- Then the system denies the action
- And nothing changes

#### US-17: Complete an exchange

*Scenario 1 - normal / valid path*

- Given the poster owns the listing
- And the request has status PICKED_UP
- When the poster marks the exchange complete
- Then the system changes the request status from PICKED_UP to COMPLETED
- And the recipient is notified

*Scenario 2 - workflow rule (wrong status)*

- Given the request is not in PICKED_UP status
- When the poster attempts to mark the exchange complete
- Then the system rejects the action
- And nothing changes

*Scenario 3 - permission rule (not the listing owner)*

- Given a member who does not own the listing
- When that member attempts to complete the exchange
- Then the system denies the action
- And nothing changes

### Reviews

#### US-18: Leave a rating and review after completion

*Scenario 1 - normal / valid path*

- Given an exchange has status COMPLETED
- And the member is the poster or the recipient for that exchange
- And the member has not already reviewed the other party
- When the member submits a rating and a short review
- Then the system saves the review
- And links it to the completed exchange
- And makes it visible to the reviewed party

*Scenario 2 - workflow rule (not completed)*

- Given an exchange has status REQUESTED, APPROVED, PICKED_UP, DENIED, or CANCELLED
- When the member attempts to submit a review
- Then the system rejects the review
- And no review is saved

*Scenario 3 - edge / error path (duplicate review)*

- Given the member has already reviewed the other party for this exchange
- When the member attempts to review again
- Then the system rejects the duplicate review

*Scenario 4 - permission rule (non-participant)*

- Given an exchange has status COMPLETED
- And the member is not the poster or the recipient for that exchange
- When the member attempts to submit a review
- Then the system denies access
- And no review is saved

#### US-19: View reviews for a completed exchange

*Scenario 1 - normal / valid path*

- Given an exchange has status COMPLETED
- And the member is the poster or the recipient for that exchange
- When the member opens the reviews for that exchange
- Then the system shows the reviews linked to it, including any review left about the viewing member

*Scenario 2 - edge / error path (no reviews yet)*

- Given a completed exchange has no reviews yet
- When a participant opens the reviews
- Then the system shows that there are no reviews yet

*Scenario 3 - permission rule (non-participant)*

- Given a member who is not a participant in the exchange
- When that member attempts to view the reviews
- Then the system denies access

### Notifications and dashboard

#### US-20: View status notifications

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

**Notes**

- Email or SMS notifications are out of scope for this story set.

#### US-28: Mark a notification as read

*Scenario 1 - normal / valid path*

- Given the member is logged in
- And the member has one or more unread notifications
- When the member opens an unread notification or marks it as read
- The system marks it as read
- And the system visually distinguishes between read/unread notifications

*Scenario 2 - edge / error path (already read)*

- Given the member is already marked read
- When the user attempts to mark as read again
- Then the system accepts silently
- And nothing changes

#### US-21: View my dashboard and activity overview

*Scenario 1 - normal / valid path*

- Given the member is logged in
- When the member opens their dashboard
- Then the system shows their active listings, incoming requests, outgoing requests, and exchange history, grouped by status
- And each item links to the action that fits its status (approve or deny a request, withdraw a request, deactivate a listing, confirm pickup, complete an exchange)

*Scenario 2 - edge / error path (no activity)*

- Given the member has no activity yet
- When the member opens their dashboard
- Then the system shows empty groups

### Admin

#### US-22: Suspend a user

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

#### US-27: Unsuspend a user

*Scenario 1 - normal / valid path*

- Given the admin is logged in with admin rights
- And the target account exists and is currently suspended
- When the admin reinstates the account
- Then the system marks the account as active
- And the account can then longer log in or take member actions

*Scenario 2 - permission rule (non-admin)*

- Given a user without admin rights
- When that user attempts to unsuspend an account
- Then the system denies the action
- And nothing changes

#### US-23: Deactivate a listing as admin

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

#### US-24: Generate basic reports

*Scenario 1 - normal / valid path*

- Given the admin is logged in with admin rights
- When the admin generates a report
- Then the system computes the report
- And shows it to the admin

*Scenario 2 - permission rule (non-admin)*

- Given a user without admin rights
- When that user attempts to generate a report
- Then the system denies the action

#### US-29: View member profile as admin

*Scenario 1 - normal / valid path*

- Given the admin is logged in with admin rights
- When the admin searches for a member by name or email
- Then the system returns matching members with their account status
- And the admin can select a member to view their full account details and active/suspension status

*Scenario 2 - edge / error path (no matches)*

- Given the admin searches for a member by name or email
- And no members match
- When the system processes the search
- The system shows an empty result

*Scenario 3 - edge / error path (suspended account*

- Given the admin selects a suspended account
- When the system loads the account details
- The system shows account details with suspended status
- And makes the reinstate action available

*Scenario 4 - permission rule (non-admin)*

- Given a user without admin rights
- When that user attempts to access the member profile view
- The system denies access

### Listing photos

#### US-25: Add and manage listing photos

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

**Notes**

- Photos are optional on a listing.
