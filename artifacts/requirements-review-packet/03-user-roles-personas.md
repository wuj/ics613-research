# User Roles and Personas

**Team:** Team 4 | Local Produce Exchange, "Surplus"\
**Course:** ICS 613 Software Engineering | University of Hawai'i at Manoa | Summer 2026\
**Members:** Kennan Kaneshiro, Shea Stevens, Matt Ong, Kim Cates, Jeff Wu

These are the roles the system defines, the context needed to read them, and the user stories where each role is the actor.

## Role context

Poster and Recipient are not separate accounts. They are the role a Member plays for a given listing or request. The same person can be a Poster on one listing and a Recipient on another. In this document, an exchange means a request together with the coordination around it, so a phrase like "the exchange has status COMPLETED" refers to that request's status. A listing's quantity available is the amount the Poster enters when creating the listing. The listing's remaining quantity starts equal to the quantity available and goes down as the Poster approves requests. A suspended account cannot log in or take any member action; this rule applies in every use case.

## Roles

- **Guest**: a person who holds an invite token but has not registered yet. A Guest can only register.
- **Member**: a registered, active user. A Member can browse, search, and filter listings, manage a profile, invite new members, post listings, submit requests, send messages, leave reviews, view notifications, and view a personal dashboard.
- **Poster**: the Member who owns a listing. A Poster creates and manages listings, handles each listing's request queue, messages the recipient, and marks a picked-up exchange complete.
- **Recipient**: the Member who submits a request for an item from a listing and, if approved, picks it up and confirms pickup. The poster reaches the recipient through the message thread.
- **Admin**: a privileged user who can suspend users, deactivate listings without deleting audit history, and generate basic reports.

## Role to story coverage

Each row maps a role to the user stories where that role is the actor.

| Role | User stories |
| --- | --- |
| Guest | US-01 |
| Member | US-02, US-03, US-04, US-05, US-06, US-07, US-30, US-12, US-18, US-19, US-20, US-28, US-21 |
| Poster | US-09, US-10, US-13, US-14, US-15, US-17, US-25 |
| Recipient | US-08, US-11, US-26, US-16 |
| Admin | US-22, US-27, US-23, US-24, US-29 |
