# Campaign Data (Reference)

[← Back to README](../README.md) · [Data Model](data-model.md) · [Admin Setup](admin-setup.md) · [User Guide](user-guide.md)

This funnel isn't hypothetical. The numbers below are the actual last-90-day
performance for the ISMP Korea Instagram account and the Linktree it feeds
into, pulled directly from Instagram's Professional Dashboard and Linktree's
Activity insights. They're the real upstream data the Salesforce data model
in this repo is built to receive.

## Instagram Performance (Last 90 Days)

| Metric | Value |
|---|---|
| Views | 216,775 |
| Accounts reached | 108,262 |
| Interactions | 797 |
| Accounts engaged | 455 |
| Profile visits | 7,139 |
| External link taps (bio → Linktree) | 919 |
| Total followers | 1,208 |
| Messaging conversations started | 48 |
| Daily response rate | 36.4% |

Reels drove the large majority of both views (51.6% among followers) and
interactions (86.7%), which is consistent with how the account's
highest-performing individual posts broke out: the top-viewed post in this
window reached 57.6K views, well ahead of the next highest at 19K.

## Linktree Performance (Last 90 Days)

| Metric | Value |
|---|---|
| Total views | 1,405 |
| Total clicks | 1,281 |
| Average click rate | 91.2% |

Weekly traffic climbed steadily from early January into a peak in the first
two weeks of March (both weeks landed around 220-230 total views), before
tapering slightly by late March.

## What This Means for the Data Model

Of the 7,139 people who visited the Instagram profile in this window, 919
tapped the link in the bio, roughly a 12.9% visit-to-click-through rate. Of
those who landed on the Linktree, 91.2% clicked through again to the actual
Google Form. That two-stage drop-off (profile → bio link → form) is exactly
why `Acquisition Channel` and `Landing Page` are separate fields on
Prospect rather than one combined field: Instagram tells you where
someone's attention started, Linktree tells you whether that attention
converted into an actual click toward the form.

One inconsistency worth noting rather than smoothing over: Instagram
reports 919 external link taps, while Linktree reports 1,405 total views
in the same window. The gap is real and expected, since Linktree traffic
can come from sources beyond the Instagram bio link (other posts, direct
shares, saved bookmarks from returning visitors), so the two numbers
aren't measuring identical populations. A data model that assumed they
should match would be modeling the tools' reporting, not the actual
funnel.

## Future Enhancement (Not Built)

<!-- TODO: keep or delete this section depending on whether you actually
     build it. -->

A natural next step would be a `Campaign` object in Salesforce that stores
these channel-level metrics as dated snapshots (weekly or monthly), so
Instagram/Linktree performance could be reported on directly alongside
Prospect conversion data instead of living in a separate reference doc.
That's out of scope for this build, but the real numbers above are already
in the right shape to seed it later: each row in the Instagram and Linktree
tables maps cleanly to a field on a future Campaign record.
