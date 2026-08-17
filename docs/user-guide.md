# User Guide

This guide is written for an Enrollment Coordinator: the person who
actually works prospects day to day, not the admin who configured the org.
It explains how to move a prospective student from first contact to paid
enrollment.

## Overview

The Course Enrollment & Conversion Tracker gives you one record per
prospect that follows the actual funnel this app is modeled on: an
Instagram post routes to a Linktree, which routes to a Google Form intake,
followed by payment and a first-week attendance check. Every update lives
on the Prospect record itself, so anyone on the team can see where a
person stands just by opening it, instead of cross-referencing a
spreadsheet against form responses.

## Logging a New Lead

A prospect becomes a Lead record the moment they're worth tracking, which
in practice is as soon as marketing spend or outreach has reached them,
even before they've filled out anything.

1. From the App Launcher (the grid icon, top left), open the **Course
   Enrollment Tracker** app.
2. Go to the **Prospects** tab and click **New**.
3. Fill in whatever contact info is available, plus **Acquisition
   Channel** (typically `Instagram`) and **Landing Page** (typically
   `Linktree`).
4. Leave **Status** set to its default, **Lead**.
5. Click **Save**.

<!-- TODO: confirm this matches your actual App Launcher / tab names once
     you've set up the app in Salesforce. -->

## Moving a Prospect Through the Funnel

Every stage change happens the same way: open the Prospect record, update
the **Status** field, and save.

### Lead → Form Submitted

Once the prospect fills out the Google Form (the intake form linked from
Linktree), transfer their answers onto the Prospect record: **School
Year**, **School**, and **Course** (whichever offering they said they're
interested in). Change **Status** to `Form Submitted` and set the **Form
Submitted Date**.

<!-- TODO: if you connect the Google Form to Salesforce directly (e.g. via
     a Zapier/Flow webhook instead of manual entry), document that
     integration here instead of the manual transfer step above. Manual
     entry is a completely legitimate way to run this at small scale. -->

### Form Submitted → Paid

Once payment comes in, don't just flip the Status field: create a new
**Payment** record related to the Prospect (Amount, Payment Date, Payment
Method), then update **Status** to `Paid`. Recording the actual payment,
not just the label, is what lets the office reconcile totals later.

### Paid → Attended Week 1

Once the prospect actually shows up to the first session, change
**Status** to `Attended Week 1` and set the **First Week Attendance
Date**. If someone paid but didn't show up, leave their Status at `Paid`
rather than moving it forward — that gap is exactly what this stage is
meant to surface.

<!-- TODO: if you built the Total Paid roll-up field, mention here that it
     updates automatically once a Payment record is saved. -->

## Checking Your Conversion Numbers

<!-- TODO: if you build the optional conversion-by-channel report from
     admin-setup.md, explain here how a non-admin user finds it (e.g. which
     tab or dashboard it lives on) and how to read it. If you skip the
     report, delete this section rather than describing one that isn't
     there. -->

## FAQ

**Q: Someone paid but never showed up for the first session. What do I do?**
A: Leave their Status at `Paid` — don't move it to `Attended Week 1`. This
is the whole reason that stage exists as its own step instead of being
assumed: it lets you report on paid-but-didn't-attend as its own group.

**Q: A prospect skipped straight from Lead to Paid without a Google Form on file. Is that a problem?**
A: It shouldn't happen in the real funnel, since the Google Form is where
School Year, School, and Course get captured — but if it does, Status can
still reflect reality. Just make sure to backfill those fields by hand
before marking them Paid, since reporting on Course and School Year will
be incomplete otherwise.

**Q: I don't see the course they're asking about in the Course lookup.**
A: The course hasn't been created yet. Ask your admin to add it under the
Courses tab before selecting it on the Prospect record.

**Q: Can one prospect make more than one payment?**
A: Yes — that's why Payment is its own related list on the Prospect
record instead of a single field. Add a new Payment record for each
transaction.

<!-- TODO: add 1-2 more once you've actually clicked through the app —
     the real questions you ask yourself in the moment are more useful
     here than ones I can guess in advance. -->
