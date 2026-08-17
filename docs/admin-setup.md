# Admin Setup Guide

[← Back to README](../README.md) · [Data Model](data-model.md) · [User Guide](user-guide.md) · [Campaign Data](campaign-data.md)

> [!NOTE]
> This is written as the plan to follow while building the org from
> scratch. After you've actually clicked through it, come back and mark
> anywhere the real Setup UI differed from what's described here — Salesforce
> shifts small things between releases, and a guide that's slightly wrong is
> more frustrating than one that's honest about what changed.

This guide walks a Salesforce admin through configuring a blank Developer
org to run the Course Enrollment & Conversion Tracker from scratch. It
assumes no prior setup beyond having the org itself.

## Prerequisites

- A Salesforce Developer Edition org ([free signup](https://developer.salesforce.com/signup))
- System Administrator profile access (the default profile for a fresh
  Developer org's first user)

## Step 1: Create the Custom Objects

Repeat this for each of the three objects: **Course**, **Prospect**, **Payment**.

1. Click the gear icon (top right) → **Setup**.
2. In the Quick Find box, type **Object Manager** and select it.
3. Click **Create** → **Custom Object**.
4. Fill in:
   - **Label**: the object name (e.g. `Course`)
   - **Plural Label**: e.g. `Courses`
   - **Object Name**: auto-fills from Label — leave it (Salesforce appends `__c`)
   - Under **Optional Features**, leave the defaults unless you have a
     specific reason to change them
5. Click **Save**.

<!-- TODO: note the order you actually created these in — Course and
     Prospect need to exist before you can add the lookup/master-detail
     fields that reference them, so build Course first, then Prospect,
     then Payment. -->

## Step 2: Add Fields

See [`data-model.md`](data-model.md) for the full field reference — add
each field listed there to its corresponding object. The relationship
fields need a little extra care:

1. On **Prospect**, add the **Course** field as a **Lookup Relationship**
   pointing to Course.
2. On **Payment**, add the **Prospect** field as a **Master-Detail
   Relationship** pointing to Prospect. (Master-Detail is its own field
   type in the "New Field" wizard, listed alongside Lookup Relationship.)
3. For each Picklist field (`Status`, `Acquisition Channel`, `Landing
   Page`, `School Year`, `Payment Method`), choose **"Enter values, with
   each value separated by a new line"** rather than using a global
   picklist, since these values are specific to this app. Use these
   values:
   - `Status`: Lead, Form Submitted, Paid, Attended Week 1
   - `Acquisition Channel`: Instagram, Referral, Organic Search, Website, Event
   - `Landing Page`: Linktree, Direct Website
   - `School Year`: Freshman, Sophomore, Junior, Senior
   - `Payment Method`: Credit Card, Bank Transfer, Cash, Other
4. After creating each field, the wizard asks which profiles get
   field-level access and whether to add it to the existing page layout —
   say yes to both for System Administrator.

<!-- TODO: if you add the Total Paid roll-up summary field from
     data-model.md, document that here too: New Field on Prospect →
     Roll-Up Summary → Summarized Object: Payment → SUM → Amount. -->

## Step 3: Configure Page Layouts

1. From each object's page in Object Manager, click **Page Layouts**.
2. Drag fields into a sensible order. On Prospect, put `Status` and
   `Acquisition Channel` near the top, since those are the fields someone
   updates most often while working a record.
3. Save each layout.

<!-- TODO: note anything you changed from the default layout and why. -->

## Step 4: Set Up Reporting (optional)

<!-- TODO: if you build a conversion-by-channel report (e.g. count of
     Prospects grouped by Acquisition Channel and Status), document the
     report type, filters, and grouping here. This is optional — skip this
     section entirely if you run out of time, rather than describing a
     report that doesn't exist. -->

## Troubleshooting

| Issue | Likely Cause | Fix |
|---|---|---|
| Can't create the Payment → Prospect field as Master-Detail | Prospect object doesn't exist yet, or Payment already has records | Create Course and Prospect first; Master-Detail can't be added to an object that already has data in most cases |
| New picklist value doesn't show up on an existing record | Field-level security or page layout wasn't updated for that profile | Re-check Step 2's "add to page layout" prompt, or add the field manually via the object's Page Layout |

<!-- TODO: replace or add rows with the real issues you actually hit —
     that's the most useful part of this table for the next person. -->
