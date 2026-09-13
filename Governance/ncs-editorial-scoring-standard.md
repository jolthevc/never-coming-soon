# Never Coming Soon
## Editorial Scoring Standard v1.2

## Purpose

The Forensic Editor returns one holistic `overall_score` from 1.0 to 10.0 in addition to component scores.

The score is an editorial judgment, not a mechanical average.

It should answer:

**How strong is this Never Coming Soon production and public edition as a complete object, and how ready is this exact draft to enter human review?**

Consider:

- production quality
- desire to watch
- story and causality
- character quality
- scene quality
- genre delivery
- format execution
- distinctiveness
- edition strategy
- voice
- readability
- spoiler calibration
- casting pleasure
- public integrity

Use one decimal place when practical.

## Calibration

- 5.0: usable material, major weaknesses remain
- 6.0: credible but clearly under the NCS quality bar
- 7.0: good, worth preserving, meaningful revision still needed
- 7.5: clearly working, but noticeable weaknesses keep it from strong publication territory
- 8.0: strong and highly watchable, limited weaknesses
- 8.5+: Gold-candidate territory when craft and production both hold
- 9.0+: rare, exceptional work
- 10.0: extraordinarily uncommon

Do not inflate scores because the production is ambitious, because the concept is personally appealing, or because revision improved it materially from an earlier draft.

## Exact-draft rule

The score saved to the Ideas row and Google Doc must come from the latest Forensic Editor review associated with the exact delivered article text.

If article text changes after the latest review, the old score is stale.

Run another editorial-review call before delivery.

Do not infer a replacement score from component scores.

Do not copy a score from an earlier draft.

Do not write `N/A`, `TBD`, or another placeholder into `ncs_score` or the Google Doc.

A missing or invalid `overall_score` is a delivery blocker.

## Score and revision-route consistency

`overall_score` and `revision_route` answer different questions, but they must not contradict each other.

A score below 8.0 means the draft still has meaningful weaknesses under this calibration.

Therefore:

- `overall_score < 8.0` may not use `revision_route = NONE`
- `overall_score >= 8.0` may still use PROSE, EDITION, or CANON when a material issue remains
- a high score never excuses a hard public-integrity, schema, or delivery failure

Do not manipulate the score to justify a desired route.

## Automatic delivery floor

The automated Generation workflow should not mark a new or replacement draft `DRAFTED` unless the exact final article receives:

- a valid numeric `overall_score`
- `overall_score >= 8.0`
- `revision_route = NONE` on the final Forensic Review
- all deterministic article QA gates passed
- all social-packet QA gates passed

This is an automated delivery floor, not a claim that every 8.0 draft should publish.

Human editorial authority remains higher.

If the workflow reaches its allowed automated revision limit and the exact final draft remains below 8.0 or still requires revision, stop without overwriting a previous successful draft. Surface the unresolved issues for the human editor or a later explicit redevelopment run.
