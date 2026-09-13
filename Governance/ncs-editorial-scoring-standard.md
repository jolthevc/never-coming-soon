# Never Coming Soon
## Editorial Scoring Standard v1.3

## Purpose

The Forensic Editor returns one holistic `overall_score` from 1.0 to 10.0 in addition to component scores.

The score is an editorial judgment, not a mechanical average and not a workflow pass/fail threshold.

It should answer:

**How strong is this Never Coming Soon production and public edition as a complete object, and how ready is this exact draft for human review?**

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
- 7.0: good, worth preserving, meaningful weaknesses remain
- 7.5: clearly working and enjoyable, with noticeable weaknesses
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

A missing or invalid numeric `overall_score` means the scoring step failed and should be repaired before normal delivery.

## Score and revision route are independent

`overall_score` describes quality.

`revision_route` describes whether another automated change is worth making now.

They are related, but there is no numeric cutoff that forces revision.

A 7.4 draft may correctly use `revision_route = NONE` when it is coherent, enjoyable, complete, and the remaining weaknesses are better left to human taste rather than another automated pass.

An 8.6 draft may still require PROSE or EDITION when it contains one material fix such as a public-integrity leak, repeated scene treatment, or a broken section.

Do not manipulate the score to justify a desired route.

Do not route to revision merely because the score is below 8.0.

## Editorial sufficiency

Never Coming Soon is not trying to auto-revise every draft into theoretical perfection.

The automated system should stop when the production is developed enough to be enjoyable, the article is coherent and readable, the genre promise is present, and no material defect is likely to be improved by another automated pass.

Minor imperfections are acceptable in a draft.

Examples that do not by themselves require another automated revision:

- one supporting character could be richer
- one paragraph is less elegant than another
- a motif could be slightly less frequent
- one episode capsule is merely good rather than excellent
- the article scores in the 7s but is clearly a developed, enjoyable idea

Examples that may justify revision:

- the central concept is not actually legible
- the genre promise is missing
- the same major scene is substantially retold across sections
- the article exposes backstage NCS terminology
- a relationship-driven story does not demonstrate the relationship at all
- a major section is broken, contradictory, or unreadable
- the draft materially misrepresents canon

The editor should prefer preserving a good object over polishing it until its personality disappears.

## Relationship to DRAFTED

`DRAFTED` is an operational lifecycle state, not an editorial award.

A production may be `DRAFTED` at 7.2, 7.6, 8.4, or another honest score if the complete generated package was successfully delivered.

The score remains useful because it tells the human editor how strong that draft is.

Publication remains a separate human decision.

Do not use score as the gate for whether a completed artifact is allowed to exist in Drive or be marked `DRAFTED`.
