# Open Questions and Assumptions

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu

The items below are not yet team-approved. Each one should be confirmed or changed before the requirements are locked.

| ID | Type | Item | Affected stories and use cases | Status |
| --- | --- | --- | --- | --- |
| OQA-01 | Open question | Token expiry rules are not yet defined. Confirm whether invite tokens expire and, if so, after how long. | US-01, UC-01 | Open |
| OQA-02 | Open question and assumption | Registration is invite-only, but who issues invite tokens is not decided. Assumed a member may issue invite tokens. Confirm whether any active member can invite, or only an admin. If the issuer is not decided, keep US-01 as core and treat US-04 as optional or pending. | US-04, UC-04 | Open |
| OQA-03 | Assumption | The poster handles requests in the order they are received. | US-10, UC-10 | Open |
| OQA-04 | Open question | Confirm that cancelling an approved request should restore the requested quantity to the listing. | US-26, UC-11 | Open |
| OQA-05 | Open question and assumption | Confirm what happens to requests already in progress on a listing that is deactivated. Assumed existing requests continue as normal and only new requests are blocked. | US-15, UC-15 | Open |
| OQA-06 | Open question | Confirm whether notifications should be deleted or marked as read. | US-28, UC-20 | Open |
| OQA-07 | Open question | Confirm the allowed photo file types and a maximum size or count before building. | US-25, UC-13, UC-14 | Open |
| OQA-08 | Assumption | The recipient confirms pickup. | US-16, UC-16 | Open |
| OQA-09 | Assumption | The poster marks the exchange complete. | US-17, UC-17 | Open |
