# Never Coming Soon
## Generation Data Contract v2.4

## 1. Purpose

This document defines Generation state and persistence for the lean Generation workflow.

Generation exists to produce a strong human-reviewable public article efficiently while also producing the canonical social handoff required by downstream media asset generation.

There is no separate Productions state table.

Generation uses:

- the `Ideas` row as durable lifecycle state
- n8n execution memory as the temporary creative workspace
- Google Drive as the finished-draft store
- GitHub as the source of truth for governance, prompts, schemas, workflow specs, and approved calibration examples

## 2. Durable identifier

Use `idea_id` as the only durable Generation identifier.

Do not create `production_id`.

## 3. Source row and eligibility

Normal entry requires:

- `status = DEVELOPMENT_SELECT`
- nonblank `development_packet_json`

An explicit `force_redevelopment=true` run may begin from an eligible `DRAFTED` row and requires explicit `idea_id`.

Generation does not use an intermediate Sheet status lock.

For a normal run, the row remains `DEVELOPMENT_SELECT` until the complete generated package succeeds.

For forced redevelopment, the row remains `DRAFTED` and the prior successful final fields remain untouched until the replacement package succeeds.

Do not write `GENERATING` during execution.

## 4. Execution-memory workspace

The following normally remain temporary only:

- source Ideas row snapshot
- parsed Development Packet
- production development blueprint
- research packet when used
- canon bible
- casting plan
- edition plan
- draft article
- deterministic diagnostics
- forensic editorial review
- IG asset packet before persistence

The normal Generation path no longer requires:

- standalone Story Challenge output
- automatic Revision Writer output
- deep rescue-loop state
- duplicate final Forensic review of unchanged prose

Do not add Sheet columns for internal stages.

## 5. Canon authority

After Canon Builder succeeds, the canon bible is authoritative for that execution.

The normal Generation path does not run a standalone Story Challenger. The Canon Builder performs an independent internal stress-test before freezing canon.

Casting, edition architecture, writing, and social packaging should represent frozen canon rather than casually reinvent it.

Foundational redevelopment after human review is a later explicit action, not an automatic Generation rescue loop.

## 6. Normal paid-model sequence

The normal paid-model path is:

1. Production Developer
2. Research Grounder only when genuinely requested
3. Canon Builder with internal challenge
4. Casting Director
5. Edition Architect
6. Edition Writer
7. Forensic Editor
8. IG Asset Packet Builder

With no research, this is seven model calls.

The Forensic Editor remains a fresh cold read.

Its `revision_route` is advisory during normal Generation. Do not automatically execute PROSE, EDITION, or CANON revision routes before human review.

## 7. Draft scoring contract

The article delivered by Generation must have a valid numeric holistic NCS score from the Forensic Editor review of that exact article.

Rules:

- 1.0 to 10.0
- preferably one decimal place
- not a component-score average
- not `N/A`
- not copied from an earlier article version

Because normal Generation does not automatically revise article prose after Forensic Review, the first Forensic score is normally the exact delivered-draft score.

Do not run a duplicate final Forensic call when article text is unchanged.

If an exceptional repair changes substantive public prose after the review, the score is stale and the repaired text must be reviewed before delivery.

The score is diagnostic, not a lifecycle threshold.

Do not require `overall_score >= 8.0`.

Do not require `revision_route = NONE`.

## 8. Deterministic article QA

Before social packaging and delivery, deterministic QA should verify at minimum:

- article nonblank
- final title nonblank
- required visible section structure for resolved format
- reasonable word count
- no em dash character
- no obvious backstage technology references
- no hard internal editorial leakage
- valid required structured objects
- major-name consistency where practical
- episode-count consistency where practical

Diagnostics should also inspect section overlap, especially THE MOVIE versus THE SCENES for film and THE SEASON versus THE EPISODES for television.

These diagnostics are cheap signals, not creative rewrite rules.

Minor overlap, rhythm, motif, casting, or prose warnings do not block completion.

## 9. Social handoff contract

A successful Generation run produces `ig_packet_json` matching:

`Schemas/ig-asset-packet.schema.json`

and governed by:

`Governance/ncs-social-asset-standard.md`

The packet is a required handoff to downstream media asset generation.

It is built from:

- final canon
- final title
- exact delivered article
- final holistic score
- resolved canonical format

The packet contains:

- campaign brief
- logo treatment
- short social caption
- locked Slide 1 cover/poster packet
- locked Slide 2 premise packet
- locked Slide 3 NCS close packet

The packet `format` field must exactly match `FILM`, `SERIES`, or `LIMITED_SERIES`.

Validate the exact final object that will be stringified into the Ideas cell after any repair, normalization, mapping, or transformation.

If IG Packet QA fails, allow one packet-only repair and validate the repaired object again.

Do not rewrite the article because the social packet alone failed.

A structurally invalid final IG packet blocks `DRAFTED` because the downstream media handoff would be incomplete.

## 10. Google Drive delivery

After article QA and final IG packet validation succeed, write the final Google Doc.

Destination pattern:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE]`

Document content:

`NCS SCORE: X.X / 10`

blank line

final article only

Do not include internal JSON, diagnostics, research, canon, review notes, or prompts.

## 11. Final Ideas update

Only after successful IG validation and Drive persistence, update the same Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` must be the final lifecycle write for a successful run.

Generation must never populate `published_url` or set `PUBLISHED`.

## 12. Meaning of DRAFTED

`DRAFTED` means:

- a complete public article exists
- the delivered article has an honest numeric NCS score
- deterministic completion checks ran
- a schema-valid final `ig_packet_json` exists
- the article was successfully persisted to Google Drive
- `final_title`, `draft_url`, `ncs_score`, and `ig_packet_json` were written successfully

`DRAFTED` does not require:

- score of 8.0 or above
- `revision_route = NONE`
- no remaining editorial notes
- automatic publication approval
- Gold quality

The lifecycle state records that the generated package exists. The score records editorial quality.

## 13. Failure behavior without an intermediate status

Normal Generation does not mutate lifecycle state until successful completion.

If a new run fails, the source row simply remains `DEVELOPMENT_SELECT`.

If forced redevelopment fails, the source row remains `DRAFTED` with the prior successful final fields and Drive artifact intact.

Do not write partial final fields during the run.

Do not write a fake or partial `draft_url`.

Do not write `N/A` into `ncs_score`.

Do not clear or replace a prior successful `ig_packet_json` until the replacement package has fully succeeded.

If duplicate-run protection is needed, solve it at the orchestration/execution level rather than by adding another durable Sheet lifecycle state.

## 14. Force redevelopment

`force_redevelopment=true` requires explicit `idea_id`.

It may regenerate an eligible `DRAFTED` production without deleting the successful existing artifact first.

Only replace final title, draft URL, score, IG packet, and status after the replacement package is successfully complete.

## 15. Later revision boundary

Human review may later trigger targeted revision or redevelopment.

Possible later actions include:

- targeted Revision Writer
- optional new Forensic review when changed prose needs a new score
- explicit EDITION redevelopment
- explicit CANON redevelopment
- regeneration of `ig_packet_json` when final public text, title, canon, or campaign direction materially changes

Do not automatically spend those premium calls on every initial generation.

## 16. Human authority

`DRAFTED` means the automated system delivered a complete article plus media-asset handoff for human judgment.

It does not mean published.

`PUBLISHED` is set only by an explicit human or publishing workflow.
