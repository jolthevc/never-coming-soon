# Never Coming Soon
## Generation Data Contract v2.3

## 1. Purpose

This document defines Generation state and persistence for the lean Draft Generation workflow.

Generation exists to produce a strong human-reviewable public article efficiently. Publishing polish and social packaging are separate later concerns.

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

Before changing status, store `pre_generation_status` in execution memory.

## 4. Start lock

Once the row is validated and the run is genuinely beginning, set the exact selected row to:

`GENERATING`

Keep this lock. It prevents duplicate work and makes active state visible.

The Error Recovery companion should restore the exact row when a run dies after the lock is written.

## 5. Execution-memory workspace

The following normally remain temporary only:

- source Ideas row snapshot
- `pre_generation_status`
- parsed Development Packet
- production development blueprint
- research packet when used
- canon bible
- casting plan
- edition plan
- draft article
- deterministic diagnostics
- forensic editorial review

The normal Draft Generation path no longer requires:

- standalone Story Challenge output
- automatic Revision Writer output
- deep rescue-loop state
- IG asset packet before `DRAFTED`

Do not add Sheet columns for internal stages.

## 6. Canon authority

After the Canon Builder, the canon bible is authoritative for that execution.

The normal Draft Generation path does not run a standalone Story Challenger. The Canon Builder performs an independent internal stress-test before freezing canon.

Casting, edition architecture, and writing should represent frozen canon rather than casually reinvent it.

Foundational redevelopment after human review is a later explicit action, not an automatic Draft Generation rescue loop.

## 7. Normal creative sequence

The normal paid-model path is:

1. Production Developer
2. Research Grounder only when genuinely requested
3. Canon Builder with internal challenge
4. Casting Director
5. Edition Architect
6. Edition Writer
7. Forensic Editor

The Forensic Editor remains a fresh cold read.

Its `revision_route` is advisory during Draft Generation. Do not automatically execute PROSE, EDITION, or CANON revision routes before human review.

## 8. Draft scoring contract

The article delivered by Draft Generation must have a valid numeric holistic NCS score from the Forensic Editor review of that exact article.

Rules:

- 1.0 to 10.0
- preferably one decimal place
- not a component-score average
- not `N/A`
- not copied from an earlier article version

Because normal Draft Generation does not automatically revise article prose after Forensic Review, the first Forensic score is normally the exact delivered-draft score.

Do not run a duplicate final Forensic call when article text is unchanged.

If an exceptional repair changes substantive public prose after the review, the score is stale and the repaired text must be reviewed before delivery.

The score is diagnostic, not a lifecycle threshold.

Do not require `overall_score >= 8.0`.

Do not require `revision_route = NONE`.

## 9. Deterministic article QA

Before delivery, deterministic QA should verify at minimum:

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

Minor overlap, rhythm, motif, casting, or prose warnings do not block `DRAFTED`.

## 10. Social handoff is deferred

Initial Draft Generation does **not** require or generate `ig_packet_json`.

The IG Asset Packet Builder and IG Packet QA belong to a later Publish Prep workflow that runs only after explicit human interest in publishing the production.

For a new Draft Generation row, `ig_packet_json` may remain blank.

For force redevelopment of an existing `DRAFTED` row, preserve any prior successful `ig_packet_json` unless and until a later Publish Prep workflow intentionally replaces it.

Do not fabricate placeholder social JSON.

## 11. Google Drive delivery

When the article passes technical completion checks, write the final Google Doc.

Destination pattern:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE]`

Document content:

`NCS SCORE: X.X / 10`

blank line

final article only

Do not include internal JSON, diagnostics, research, canon, review notes, or prompts.

## 12. Final Ideas update

Only after successful Drive persistence, update the same Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `status = DRAFTED`

For a new draft, leave `ig_packet_json` blank.

For force redevelopment, preserve an existing successful `ig_packet_json` unless a separate Publish Prep flow replaces it.

`DRAFTED` must be the last success-state write.

Generation must never populate `published_url` or set `PUBLISHED`.

## 13. Meaning of DRAFTED

`DRAFTED` means:

- a complete public article exists
- the delivered article has an honest numeric NCS score
- deterministic completion checks ran
- the article was successfully persisted to Google Drive
- `final_title`, `draft_url`, and `ncs_score` were written successfully

`DRAFTED` does **not** require:

- an IG packet
- score of 8.0 or above
- `revision_route = NONE`
- no remaining editorial notes
- automatic publication approval
- Gold quality

The lifecycle state records that the draft artifact exists. The score records editorial quality. Social readiness is a separate later concern.

## 14. Failure recovery

If a run fails after setting `GENERATING` but before successful draft delivery:

- do not write a fake or partial `draft_url`
- do not write `N/A` into `ncs_score`
- do not replace successful prior final fields
- restore the exact row to `pre_generation_status` only when its current status is still `GENERATING`

For a normal new run:

`pre_generation_status = DEVELOPMENT_SELECT`

For force redevelopment:

`pre_generation_status = DRAFTED`

Preserve prior successful `final_title`, `draft_url`, `ncs_score`, `ig_packet_json`, `human_notes`, `published_url`, and Drive artifact until replacement delivery succeeds.

## 15. Force redevelopment

`force_redevelopment=true` requires explicit `idea_id`.

It may regenerate an eligible `DRAFTED` production without deleting the successful existing artifact first.

Only replace final title, draft URL, score, and status after the new draft is successfully persisted.

Do not clear an existing IG packet as a side effect of Draft Generation.

## 16. Publish Prep boundary

Publish Prep is a separate, explicit later workflow.

It may load a `DRAFTED` article plus its Forensic review and then run, only when desired:

- targeted Revision Writer
- optional final Forensic review when revised prose needs a new score
- IG Asset Packet Builder
- IG Packet QA
- future image/social handoff

CANON or EDITION redevelopment should require explicit human action rather than automatic Draft Generation looping.

## 17. Human authority

`DRAFTED` means the automated system delivered a draft for human judgment.

It does not mean published.

`PUBLISHED` is set only by an explicit human or publishing workflow.
