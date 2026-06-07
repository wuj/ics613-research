# Open Questions and Assumptions

**Team 4** | Surplus (Local Produce Exchange)
ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026
Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu

The items below are not yet team-approved. Each one should be confirmed or changed before the requirements are locked.

| ID | Type | Item | Affected stories and use cases | Status |
| --- | --- | --- | --- | --- |
| OQA-01 | Open question | Token expiry rules are not yet defined. Confirm whether invite tokens expire and, if so, after how long. | [US-01](01-user-stories.md#us-01-register-with-an-invite-token), UC-01 | Open |
| OQA-02 | Open question and assumption | Registration is invite-only, but who issues invite tokens is not decided. Assumed a member may issue invite tokens. Confirm whether any active member can invite, or only an admin. If the issuer is not decided, keep [US-01](01-user-stories.md#us-01-register-with-an-invite-token) as core and treat [US-04](01-user-stories.md#us-04-invite-a-new-member) as optional or pending. | [US-04](01-user-stories.md#us-04-invite-a-new-member), UC-04 | Open |
| OQA-03 | Assumption | The poster handles requests in the order they are received. | [US-10](01-user-stories.md#us-10-approve-or-deny-a-pending-request), UC-10 | Open |
| OQA-04 | Open question | Confirm that cancelling an approved request should restore the requested quantity to the listing. | [US-26](01-user-stories.md#us-26-cancel-an-approved-request), UC-11 | Open |
| OQA-05 | Open question and assumption | Confirm what happens to requests already in progress on a listing that is deactivated. Assumed existing requests continue as normal and only new requests are blocked. | [US-15](01-user-stories.md#us-15-deactivate-own-listing), UC-15 | Open |
| OQA-06 | Open question | Confirm whether notifications should be deleted or marked as read. | [US-28](01-user-stories.md#us-28-mark-a-notification-as-read), UC-20 | Open |
| OQA-07 | Open question | Confirm the allowed photo file types and a maximum size or count before building. | [US-25](01-user-stories.md#us-25-add-and-manage-listing-photos), UC-13, UC-14 | Open |
| OQA-08 | Assumption | The recipient confirms pickup. | [US-16](01-user-stories.md#us-16-confirm-pickup), UC-16 | Open |
| OQA-09 | Assumption | The poster marks the exchange complete. | [US-17](01-user-stories.md#us-17-complete-an-exchange), UC-17 | Open |
