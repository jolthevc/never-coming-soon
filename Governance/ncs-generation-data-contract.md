# Never Coming Soon
## Generation Data Contract v2.0

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

## 4. Start lock

Once the row is validated and the workflow is genuinely starting:

`DEVELOPMENT_SELECT` -> `GENERATING`

The primary purpose of `GENERATING` is to prevent accidental duplicate work and make active state visible.

Do not set it during a dry structural test that never begins a real run unless the test also restores the row immediately.

## 5. Execution-memory workspace

The following artifacts normally remain in n8n execution memory only:

- source Ideas row snapshot
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

## 8. Final scoring contract

The final delivered draft must have a valid numeric holistic NCS score.

The score must come from `editorial_review_json.overall_score` for the exact draft being delivered.

Rules:

- 1.0 to 10.0
- preferably one decimal place
- not a component-score average
- not `N/A`
- not copied from an earlier draft review after prose changed

If any revision changes article text after the latest review, run another Forensic Editor call before delivery.

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

Do not persist the packet if the run fails before successful delivery.

## 11. Google Drive delivery

Only after the final article, final score, deterministic QA, and IG packet are valid should Generation write the final Google Doc.

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

## 13. Failure handling

If a run fails after setting `GENERATING` but before successful final delivery:

- do not set `DRAFTED`
- do not write a fake or partial `draft_url`
- do not write `N/A` into `ncs_score`
- do not replace successful prior final fields during a failed force-redevelopment run
- reset the exact failed row from `GENERATING` to `DEVELOPMENT_SELECT` when appropriate

A companion n8n Error Trigger workflow is the preferred cleanup mechanism.

The error workflow must identify the exact failed `idea_id` and only reset the row when its current status is still `GENERATING`.

Never reset an arbitrary row merely because it is GENERATING.

## 14. Force redevelopment

`force_redevelopment=true` requires explicit `idea_id`.

It may regenerate a `DRAFTED` production without deleting the successful existing artifact first.

Preserve:

- human notes
- source Ideation fields
- published URL
- previous Drive artifact until replacement succeeds

Only replace final title, draft URL, score, IG packet, and status after the new delivery passes every success gate.

## 15. Human authority

`DRAFTED` means the automated system has delivered a complete draft and asset handoff.

It does not mean published.

The human editor remains the final publication authority.

`PUBLISHED` is set only by an explicit human or publishing workflow.
