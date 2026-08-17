# Data Model

<!-- TODO: This whole file describes the object model. Fill it in with what
     you actually build in Salesforce — don't leave placeholder field names
     in the final version. It's fine (expected, even) if your real build
     differs from this starting sketch. -->

This document describes the objects, fields, and relationships behind the
Course Enrollment & Conversion Tracker.

## Objects

| Object | Type | Purpose |
|---|---|---|
| Lead (or a custom "Prospect" object) | [Standard / Custom] | A person who has shown interest but hasn't submitted a formal inquiry yet |
| Inquiry | Custom | A formal request for information tied to a specific course or program |
| Enrollment | Custom | Confirms a prospect has committed to a course |
| Payment | Custom (or a field on Enrollment) | Tracks whether and how much a prospect has paid |

<!-- TODO: adjust the table above to match your real object names. Decide
     early whether you're extending the standard Lead object or building a
     fully custom object model — either is defensible, but the doc should
     explain which you picked and why (see Design Decisions below). -->

## Field Reference

### [Object Name]

| Field | Type | Description |
|---|---|---|
| Status | Picklist | Where this record sits in the funnel: Lead, Inquiry, Enrolled, Paid |
| Acquisition Channel | Picklist | Where the prospect came from (e.g. Instagram, Referral, Web) |
| [Add real fields] | | |

<!-- TODO: repeat this table per object. Include every field that matters
     to how the app works, not just the obvious ones — this is the section
     an engineer or another admin would actually reference. -->

## Relationships

```
Lead ──▶ Inquiry ──▶ Enrollment ──▶ Payment
```

<!-- TODO: replace with an accurate diagram once the real relationships
     (lookup vs. master-detail, 1:1 vs 1:many) are decided. A simple text
     diagram like the one above is fine — clarity matters more than a fancy
     rendered image. -->

## Design Decisions

<!-- TODO: 2-4 short paragraphs explaining choices you made and why, e.g.:
     - Why a custom object instead of extending Lead
     - Why Status is a picklist instead of four boolean fields
     - Any tradeoff you considered and rejected

     This section is what shows a reader you understand the platform, not
     just that you can fill out a form — don't skip it. -->
