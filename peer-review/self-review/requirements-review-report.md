# Requirements Review Report

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu\
**Reviewed team:** Team 4 | Local Produce Exchange, "Surplus" (self-review)

## Summary

We reviewed the requirements packet for the Local Produce Exchange project, "Surplus". The packet holds a cover page and five content documents: the user stories document (30 stories, US-01 through US-30), the acceptance criteria document (a scenario set for every story), the roles document, the open questions and assumptions document (nine numbered items, OQA-01 through OQA-09), and the known risks document (seven risks with mitigations). All five items the assignment asks for are present. This is a self-review: the team applied the same review process to its own packet that it applies to another team's packet.

The packet is in good shape overall. Stories follow one uniform format with an explicit actor, an observable action, and a stated benefit. Acceptance criteria (the pass-or-fail checks that say when a story is done) are written as Given/When/Then scenarios with concrete numbers, and nearly every story carries at least one error path and one permission rule. The problems we found are small in size but large in effect: two garbled sentences land inside core workflow rules, one scenario contradicts the request lifecycle, and two stories give conflicting answers about who may see reviews.

The findings below group into strengths worth keeping, wording that can be read more than one way, situations no scenario decides, and features whose size is not yet bounded.

## Strengths

**F-01:** Every story uses the same shape: an actor, an observable action, and a stated benefit, plus a priority, a source use case, and a milestone. The count, 30 stories, sits at the top of the assignment's 25 to 30 range, and the stories read as distinct functionalities rather than one feature split into variants.

**F-02:** The acceptance criteria are testable as written. They use concrete quantities that decide pass or fail: a listing "has 10 units available", a request for 3 units creates a queue entry, a request for 12 is rejected, and a request for 0 draws a validation error (US-08, pages 4 to 5 of the acceptance criteria document). Empty states are also covered: an empty search result (US-06), an empty request queue (US-09), an empty notification list (US-20), and an empty dashboard (US-21).

**F-03:** Permissions are captured the way the course asks: as scenarios inside acceptance criteria rather than as separate stories. Nearly every story has a deny path for the wrong person. A non-owner cannot approve a request ("Given a member who does not own the listing", "Then the system denies access", US-10, page 6), a non-party cannot open a message thread (US-12), a non-admin cannot suspend an account (US-22), and suspended accounts are blocked across actions (US-02, US-04, US-08, US-13, US-14).

**F-04:** The roles document matches the assignment's baseline roles: member, poster as the owner role, admin, and guest, with recipient as a per-request role. It also includes a role-to-story coverage table, so a reader can confirm in one place that every role has stories and every story has a clear role.

**F-05:** The open questions and the risks tie back to specific stories. Each open question names the affected story and use case IDs, and the riskiest business rules in the risks document name the exact stories whose criteria test them: "over-claim prevention (US-08, US-10), status transitions (US-10, US-11, US-26, US-16, US-17), and no cancellation after pickup (US-11, US-26)" (page 1 of the known risks document).

## Ambiguities

These are requirements that can be read in more than one way.

**F-06, high:** US-26's rejection scenario reads "Given the request is in terminal status PICKED_UP, DENIED, CANCELLED" (page 7 of the acceptance criteria document). Calling PICKED_UP terminal contradicts US-17, where "the system changes the request status from PICKED_UP to COMPLETED" (page 10), and it contradicts the course project description, which defines the sequence REQUESTED to APPROVED to PICKED_UP to COMPLETED. The list also omits COMPLETED, so no scenario rejects a cancel attempt on a finished exchange, and a cancel attempt on a REQUESTED request is left undecided. Which statuses must reject a cancel attempt, and is PICKED_UP meant to appear in that list even though it is not terminal?

**F-07, high:** US-30 shows any logged-in member another member's "display name and their review history" (page 4 of the acceptance criteria document). US-19 denies review access to outsiders: "Given a member who is not a participant in the exchange", "Then the system denies access" (page 12). Read together, a member can reach reviews through a public profile that the exchange view would deny them. Does "review history" mean full review text, or only a summary such as an average rating?

**F-08, medium:** US-26's restore step reads "Restaurants the quantity back to the listing's quantity" (page 7 of the acceptance criteria document). "Restaurants" appears to be a typo for "Restores", and "the listing's quantity" does not say which quantity, since the roles document separates "quantity available" from "remaining quantity". OQA-04 already asks about this rule, but the criterion as written cannot be tested. Should the step read "Restores the approved quantity back to the listing's remaining quantity"?

**F-09, medium:** Submission and approval check different quantities. US-08 rejects a request for 12 because "the listing has 10 units available" (page 5 of the acceptance criteria document), while approval checks "the remaining quantity" (US-10, page 6) and the queue view shows "the quantity remaining" (US-09, page 5). If 6 of 10 units are already approved, may a new member still request 8? State whether the cap at submission time is the original quantity available or the current remaining quantity.

**F-10, medium:** US-08's duplicate rule reads "Given the recipient already has an open request on this listing" (page 5 of the acceptance criteria document). "Open" is not defined, and other scenarios say "pending" instead. Does open mean REQUESTED only, or REQUESTED and APPROVED? Under the narrow reading, a member with an approved pickup can file a second request on the same listing.

**F-11, medium:** US-27's outcome reads "And the account can then longer log in or take member actions" (page 14 of the acceptance criteria document). A word is missing, and the two likely repairs point in opposite directions: "can no longer log in" blocks the account, while "can then log in" restores it. The neighboring step, "marks the account as active", suggests the second reading, but the sentence that states the user-facing outcome should not need repair. Which wording is intended?

**F-12, medium:** US-04 gates invites on a rule no story defines: "Given the member is not allowed to issue invites" (page 2 of the acceptance criteria document). Nothing in the packet says who or what makes a member allowed to invite, and OQA-02 records the underlying decision as still open while US-04 sits in the current milestone. Who grants or revokes the ability to invite, and is a story needed to manage it?

## Missing Cases

These are situations where no scenario decides the outcome.

**F-16, medium:** The first milestone depends on features planned for the second. Browsing, requesting, approving, and messaging (US-06 through US-12) are marked "Current scope (R1)", but creating a listing (US-13) is marked "Full features (R2)", so the R1 stories operate on listings that nothing in R1 creates. R1 criteria also assert notifications, for example "the poster is notified of the new request" (US-08, page 4 of the acceptance criteria document), while viewing notifications (US-20) is R2. How will the R1 stories be exercised: seeded data, an early slice of US-13, or a milestone change?

**F-17, medium:** Suspension stops logins, but no scenario says what happens to a suspended member's open work: their active listings, the queued or approved requests on those listings, or their own approved pickups on other members' listings. The open questions document does not raise it either. When an account is suspended, are its listings hidden, and are its in-flight requests cancelled or frozen?

**F-18, medium:** Every listing requires a pickup window (US-13), but no scenario or open question covers the window passing: whether the listing stays browsable, whether queued requests expire, and whether an approved but never picked-up request holds the remaining quantity forever. What happens to a listing and its requests when the pickup window ends?

**F-19, medium:** Photos can be added, replaced, and removed (US-25) "so that members can see what the item looks like", but no story displays them: US-07 notes that "Photos are not part of the current listing detail view" (page 4 of the acceptance criteria document). As written, members can manage photos that no one can see. Which story shows photos to browsing members, or should the US-25 benefit change?

## Scope Risks

These are features whose size or difficulty is not yet bounded.

**F-21, medium:** US-24 asks for "a basic report about system activity", and its criteria say only that the system "computes the report" and shows it to the admin (page 14 of the acceptance criteria document). Nothing names the reports, their contents, or their filters, so the feature has no defined stopping point. Which two or three concrete reports count as basic for the first release?

**F-22, medium:** Photo upload is the one feature in the packet that needs file handling, storage decisions, and size limits, and the known risks document does not mention it. OQA-07 asks about allowed file types and a maximum size or count, which addresses part of the exposure. Where will photos be stored and served, and should media handling join the risks list with a mitigation?
