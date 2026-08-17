# Data Model

[← Back to README](../README.md) · [Admin Setup](admin-setup.md) · [User Guide](user-guide.md) · [Campaign Data](campaign-data.md)

> [!IMPORTANT]
> This is the object model this project is built to. If your actual build
> in Salesforce ends up differing in any small way, update this file to
> match reality before you consider it finished — a data model doc that
> doesn't match the org is worse than no doc at all.

This document describes the objects, fields, and relationships behind the
Course Enrollment & Conversion Tracker: three custom objects that model a
prospect moving through a course enrollment funnel, from first contact to
payment.

## Objects

| Object | Type | Purpose |
|---|---|---|
| Course | Custom | A course or program a prospect can enroll in |
| Prospect | Custom | A person moving through the funnel: Lead → Form Submitted → Paid → Attended Week 1 |
| Payment | Custom | One payment transaction tied to a Prospect (a prospect can have more than one) |

## Field Reference

### Course

| Field | Type | Description |
|---|---|---|
| Course Name | Text (80) | The Name field — e.g. "Business English Fundamentals" |
| Price | Currency | List price for the course |
| Start Date | Date | When the course begins |
| Description | Long Text Area | Optional — a sentence or two on what the course covers |

### Prospect

The fields below follow the actual funnel I ran at ISMP Korea: an Instagram
post or ad drives traffic to a Linktree, which routes to a Google Form that
captures school year, school, and which course the prospect wants, followed
by payment and a first-week attendance check.

| Field | Type | Description |
|---|---|---|
| Prospect Name | Text (80) | The Name field — the person's full name |
| Email | Email | Contact email |
| Phone | Phone | Contact phone number |
| Status | Picklist (Lead, Form Submitted, Paid, Attended Week 1) | Where this record sits in the funnel. Defaults to Lead. |
| Acquisition Channel | Picklist (Instagram, Referral, Organic Search, Website, Event) | The top-level channel the prospect came from |
| Landing Page | Picklist (Linktree, Direct Website) | The specific link that routed them to the intake form — kept separate from Acquisition Channel because the same Instagram post might route through different links over time |
| School Year | Picklist (Freshman, Sophomore, Junior, Senior) | Captured from the Google Form at signup |
| School | Text (120) | Captured from the Google Form at signup — free text, since the pool of schools isn't fixed |
| Course | Lookup (Course) | Which course/offering the prospect selected on the Google Form |
| Form Submitted Date | Date | When Status first moved to Form Submitted |
| First Week Attendance Date | Date | When Status first moved to Attended Week 1 — left blank if they paid but never showed up |
| Notes | Long Text Area | Optional freeform notes from whoever's working the record |
| Total Paid | Roll-Up Summary (SUM of Payment.Amount) | Read-only — automatically totals every related Payment record |

### Payment

| Field | Type | Description |
|---|---|---|
| Prospect | Master-Detail (Prospect) | The prospect this payment belongs to |
| Amount | Currency | Payment amount |
| Payment Date | Date | When the payment was received |
| Payment Method | Picklist (Credit Card, Bank Transfer, Cash, Other) | Optional — how the prospect paid |

<!-- TODO: if you add or rename any field while actually building this,
     update the tables above to match. -->

## Relationships

```
Course (1) ──────< (many) Prospect (1) ──────< (many) Payment
   lookup                          master-detail
```

- **Course → Prospect** is a lookup relationship: a course can have many
  interested prospects, but a prospect isn't required to have a course
  chosen right away (e.g. early-stage leads who haven't picked one yet).
- **Prospect → Payment** is a master-detail relationship: a payment record
  has no meaning without its parent prospect, and master-detail is what
  makes the roll-up summary below possible.

## Design Decisions

I built three custom objects instead of extending Salesforce's standard
Lead object. Lead is the more common real-world choice for this kind of
top-of-funnel tracking, but it comes with built-in conversion behavior
(Lead → Contact/Account/Opportunity) that adds complexity I didn't need for
a project built to be understood end to end in a single sitting. Custom
objects also meant I got hands-on practice with the full Object Manager
workflow: creating objects, fields, and relationships from a blank org,
which is closer to what I'd actually be documenting in a real admin guide.

I used one `Status` picklist on Prospect instead of four separate stage
objects (or four boolean fields) because the funnel is inherently
sequential — a prospect is in exactly one stage at a time, and a picklist
makes that constraint explicit instead of leaving it to be enforced by
convention.

I made Prospect → Payment a master-detail relationship rather than a
lookup, specifically so I could add a roll-up summary field on Prospect
(`Total Paid`, summing `Payment.Amount`). That single decision is a good
example of why the relationship type matters beyond just "which object
owns which": lookup relationships can't power roll-up summaries, so the
choice here was driven by a feature I wanted, not just data ownership.



The `Status` values (Lead, Form Submitted, Paid, Attended Week 1) match the
actual funnel this models rather than a generic CRM template. I kept
`Acquisition Channel` and `Landing Page` as two separate fields instead of
one, because they answer different questions a real marketing team would
ask: Channel tells you *where* someone saw the program (Instagram), while
Landing Page tells you *which specific link* they clicked through
(Linktree). The same Instagram account might route traffic through more
than one link over time, so collapsing those into a single field would
lose information. This isn't a hypothetical concern: the real 90-day data
in [`campaign-data.md`](campaign-data.md) shows Instagram profile visits,
external link taps, and Linktree views as three different, non-matching
numbers, which is exactly the kind of drop-off a single combined field
would hide.

`School Year`, `School`, and `Course` are only ever populated once a
prospect reaches `Form Submitted`, because that's the actual point in the
real funnel where that information exists — it's collected on the Google
Form, not before. I added `First Week Attendance Date` as its own field
(and its own funnel stage) rather than assuming payment equals success:
someone can pay and still not show up to the first session, and that gap
between paying and attending is exactly the kind of signal a real program
would want to track separately.
