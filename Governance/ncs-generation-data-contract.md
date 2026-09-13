# Never Coming Soon
## Generation Data Contract v2.1

## 1. Purpose

This document defines Generation state and persistence.

The architecture is intentionally lean.

There is no separate Productions state table in v2.

Generation uses:

- the `Ideas` row as durable lifecycle state
- n8n execution memory as the temporary creative workspace
- Google Drive as the finished-draft store
- GitHub as the source of truth for governance, prompts, schemas, workflow specs, and approved calibration examples

## 2. Durable identifier

Use `idea_id` as the only durable Generation identifier.

Do not create `production_id`.

Do not derive a second identity namespace from the idea.

## 3. Source row

Generation begins from exactly one Ideas row.

Normal entry requirement:

- `status = DEVELOPMENT_SELECT`
- nonblank `development_packet_json`

An explicit force-redevelopment run may also begin from an eligible `DRAFTED` row.

Before changing status, store `pre_generation_status` in execution memory.

## 4. Start lock

Once the row is validated and the workflow is genuinely starting, set the exact selected row to:

`GENERATING`

The purpose of `GENERATING` is to prevent duplicate work and make active state visible.

Do not set it during a dry structural test that never begins a real run unless the test also restores the row immediately.

## 5. Execution-memory workspace

The following artifacts normally remain in n8n execution memory only:

- source Ideas row snapshot
- `pre_generation_status`
- parsed Development Packet
- production development blueprint
- research packet
- story challenge
- canon bible
- casting plan
- edition plan
- draft versions
- deterministic diagnostics
- editorial reviews
- revision outputs
- final IG asset packet before persistence

Do not add Sheet columns for these internal stages.

Do not recreate durable checkpoint/resume architecture unless repeated operational evidence later justifies it.

## 6. Canon authority

After the Canon Builder, the current canon bible is authoritative for the rest of that execution.

Casting, edition architecture, writing, revision, and social packaging should represent that canon rather than casually reinvent it.

A controlled CANON rescue may reopen canon once when Forensic Review identifies a foundational production problem.

## 7. Revision routes

The Forensic Editor may return:

- `NONE`
- `PROSE`
- `EDITION`
- `CANON`

Use the shallowest route that can solve the problem.

Limit automated deep rescue to one reentry cycle.

A later final review always scores the exact final draft that will be delivered.

## 8. Final scoring and quality contract

The final delivered draft must have a valid numeric holistic NCS score from the exact final article review.

Rules:

- 1.0 to 10.0
- preferably one decimal place
- not a component-score average
- not `N/A`
- not copied from an earlier draft after prose changed

If any revision changes article text after the latest review, run another Forensic Editor call before delivery.

For automatic `DRAFTED` delivery, also require:

- `overall_score >= 8.0`
- final `revision_route = NONE`

A score below 8.0 represents meaningful remaining weakness under the scoring standard and may not be treated as automatic completion.

If the allowed automated revision path ends below 8.0 or with a non-NONE final route, stop before Drive replacement and final state update.

## 9. Final public-integrity contract

Before delivery, deterministic QA must verify at minimum:

- final article is nonblank
- final title is nonblank
- no em dash character
- no obvious backstage technology references
- no hard internal editorial leakage from `Governance/ncs-publication-integrity-standard.md`
- required visible section structure for the resolved format
- reasonable word count
- major-name consistency where practical
- episode-count consistency where practical

Diagnostics should also inspect section overlap, especially THE MOVIE versus THE SCENES for film and THE SEASON versus THE EPISODES for television.

Section-overlap diagnostics are heuristic evidence for the Forensic Editor, not an automatic rewrite rule.

Internal-language leakage is not merely a cosmetic warning. Exact hard-leak terms should normally block delivery until revised.

## 10. Social handoff contract

A successful Generation run also produces `ig_packet_json` matching:

`Schemas/ig-asset-packet.schema.json`

and governed by:

`Governance/ncs-social-asset-standard.md`

The packet is built from final canon and the exact final article.

It contains:

- campaign brief
- logo treatment
- short social caption
- locked Slide 1 cover/poster packet
- locked Slide 2 premise packet
- locked Slide 3 NCS close packet

The packet `format` field must exactly match the canonical uppercase enum:

- `FILM`
- `SERIES`
- `LIMITED_SERIES`

Validate the exact final object that will be stringified into the Ideas cell.

If orchestration repairs, normalizes, maps, or transforms the model output, validate the post-transformation object again.

A raw model response passing schema validation does not validate a later modified object.

Schema failure is a delivery blocker.

Do not persist the packet if the run fails before successful delivery.

## 11. Google Drive delivery

Only after the final article, final review, quality floor, deterministic article QA, and exact final IG packet are valid should Generation write the final Google Doc.

Destination pattern:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE]`

Document content:

`NCS SCORE: X.X / 10`

blank line

final article only

Do not include internal JSON, diagnostics, research, canon, review notes, or prompts in the document.

## 12. Final Ideas update

Only after successful Drive persistence, update the same Ideas row:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` must be the last success-state write.

Generation must never populate `published_url` or set `PUBLISHED`.

## 13. Failure and quality-gate recovery

If a run fails or fails the final quality gate after setting `GENERATING` but before successful final delivery:

- do not set `DRAFTED` as a new success
- do not write a fake or partial `draft_url`
- do not write `N/A` into `ncs_score`
- do not replace successful prior final fields
- restore the exact row to its `pre_generation_status` when current status is still `GENERATING`

For a normal new run:

`pre_generation_status = DEVELOPMENT_SELECT`

so recovery is:

`GENERATING` -> `DEVELOPMENT_SELECT`

For force redevelopment of an existing successful draft:

`pre_generation_status = DRAFTED`

so recovery is:

`GENERATING` -> `DRAFTED`

while preserving the prior successful `final_title`, `draft_url`, `ncs_score`, `ig_packet_json`, `human_notes`, and `published_url`.

A companion n8n Error Trigger workflow is the preferred cleanup mechanism.

The error workflow must recover the exact failed `idea_id`, confirm the row is still `GENERATING`, and restore the correct prior status. Never reset an arbitrary GENERATING row.

## 14. Force redevelopment

`force_redevelopment=true` requires explicit `idea_id`.

It may regenerate a `DRAFTED` production without deleting the successful existing artifact first.

Preserve:

- human notes
- source Ideation fields
- published URL
- previous final title
- previous draft URL
- previous score
- previous IG packet
- previous Drive artifact

Only replace final title, draft URL, score, IG packet, and status after the new delivery passes every success gate.

## 15. Human authority

`DRAFTED` means the automated system has delivered a complete draft and asset handoff that cleared the automated quality floor.

It does not mean published.

The human editor remains the final publication authority.

`PUBLISHED` is set only by an explicit human or publishing workflow.
