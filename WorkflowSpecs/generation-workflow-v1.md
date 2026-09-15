# Never Coming Soon
## n8n Generation Workflow Specification v2.3

> Filename retained for loader compatibility.

## 1. Objective

Build one clean Draft Generation workflow that starts from one eligible Ideas row and ends with:

- a complete Never Coming Soon article in Google Drive
- a valid holistic NCS score for that exact delivered article
- the same Ideas row set to `DRAFTED`

Draft Generation is intentionally narrower than Publish Prep.

It develops the production, writes the article, cold-reviews it once, and delivers it for human judgment.

It does **not** automatically revise, rescue, create social assets, or publish.

## 2. Core principle

Spend premium model calls where they create creative quality.

Normal paid-model path:

1. Production Developer
2. Research Grounder only when genuinely requested
3. Canon Builder with internal independent challenge
4. Casting Director
5. Edition Architect
6. Edition Writer
7. Forensic Editor

With no research, this is six creative/model calls.

Do not add another prose/humanization node.

## 3. Systems and state

- GitHub: source of truth for governance, prompts, schemas, Gold examples, and this spec
- Google Sheet `Never Coming Soon — Ideas`: catalog and durable lifecycle state
- Google Drive: finished human-readable draft store
- n8n execution memory: temporary workspace
- n8n Code nodes: deterministic routing, diagnostics, JSON handling, QA, and state updates

There is no required Productions tab and no `production_id`.

## 4. Source snapshot

At the beginning of each live run:

1. resolve current GitHub `main` commit SHA
2. store as `source_commit_sha`
3. fetch every runtime file used by that execution at the same pinned SHA

Do not mix prompt/governance versions inside one execution.

## 5. Stage-specific runtime loading

Do not load one giant governance bundle into every model call.

Load only the files relevant to each stage.

### Production Developer

Use:

- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-generation-review-revision-os.md`
- `Governance/ncs-story-development-standard.md`
- relationship standard when relevant
- research grounding standard when relevant
- `Prompts/Generation/01-production-developer.*`
- `Schemas/production-development.schema.json`

### Research Grounder, conditional

Use only:

- research grounding governance
- stage 02 prompt
- research schema
- exact research questions

### Canon Builder

Use:

- brand
- generation OS
- story development standard
- relationship standard when relevant
- research grounding standard
- stage 04 prompt
- canon schema
- Development Packet
- Production Developer output
- research packet if any
- human notes

Do not pass a Story Challenger object in the normal path.

### Casting Director

Use:

- canon
- casting standard
- stage 05 prompt
- casting schema
- compact casting history summary

Do not send Gold articles.

### Edition Architect

Use:

- canon
- casting
- editorial anatomy
- television standard when relevant
- relationship standard when relevant
- voice/quality guidance only as needed for planning
- publication-integrity standard
- stage 06 prompt
- edition-plan schema

### Edition Writer

Use:

- canon
- casting
- edition plan
- voice constitution
- editorial anatomy
- television standard when relevant
- relationship standard when relevant
- publication-integrity standard
- editorial-quality guidance
- stage 07 prompt
- edition-draft schema
- approved Gold examples

### Forensic Editor

Use:

- canon
- edition plan
- exact draft
- deterministic diagnostics
- editorial-quality standard
- scoring standard
- publication-integrity standard
- format-specific governance when relevant
- stage 08 prompt
- editorial-review schema

Do not send visual or social-asset governance to article-generation stages.

Keep stable prompt prefixes identical where practical so provider prompt caching can work.

## 6. Structured-output safety

Every provider response schema must have a plain root object with `type = object` and `additionalProperties = false`.

At root, do not use:

- `oneOf`
- `anyOf`
- `allOf`
- `enum`
- `const`
- `not`

Nested enums are allowed when supported.

If model output is invalid JSON or schema-invalid, allow one structure-only repair using the original response plus exact validation error.

If repair fails, stop downstream stages.

## 7. Trigger and eligibility

Inputs:

- `idea_id`: optional string
- `force_redevelopment`: optional boolean, default false

If `idea_id` is supplied, find exactly that row.

If blank, select the first row top-to-bottom where:

- `status = DEVELOPMENT_SELECT`
- `development_packet_json` is nonblank

Process exactly one idea per execution.

Eligibility:

- DEVELOPMENT_SELECT: eligible
- GENERATING: stop
- DRAFTED: require explicit force redevelopment
- PUBLISHED: stop

Force redevelopment requires explicit `idea_id` and may run on DEVELOPMENT_SELECT or DRAFTED.

Do not require the source `format` cell to be populated.

## 8. Start lock and failure recovery

Keep the intermediate `GENERATING` lock.

Before changing status:

`pre_generation_status = current row status`

Then update the exact selected row by `idea_id` to:

`GENERATING`

Preserve source row snapshot, `human_notes`, `published_url`, and prior successful final fields in execution memory.

The Error Recovery companion must remain active.

If the run fails before successful draft delivery:

- recover the exact `idea_id`
- only restore when current status is still `GENERATING`
- restore to `pre_generation_status`

Normal failure restores DEVELOPMENT_SELECT.

Failed forced redevelopment restores DRAFTED and preserves the prior artifact and final fields.

Never reset an arbitrary GENERATING row.

## 9. Build source bundle

Parse `development_packet_json`.

Build a compact execution context.

Also create `casting_history_summary` from approved Gold casts at minimum. Include recent DRAFTED cast history only when it is already cheap to obtain.

Do not create another durable casting database.

## 10. Production Developer

Run the current Production Developer.

Inputs:

- Development Packet
- human notes
- relevant governance only

Output:

`production_development`

Use a strong creative/reasoning model.

## 11. Conditional research

Decision signal:

nonempty `production_development.research_requests`

If empty, skip research entirely.

If nonempty, research only those questions.

Do not run generic research merely because the setting is real.

## 12. No standalone Story Challenger

Remove or bypass the live paid Story Challenger node from the normal path.

Do not call stage 03 or require `Schemas/story-challenge.schema.json` in normal Draft Generation.

The files may remain in GitHub for history/future experiments.

Quality protection moves into the Canon Builder prompt.

## 13. Canon Builder with internal challenge

Build `canon_bible` from:

- Development Packet
- Production Developer output
- research packet if any
- human notes

The current Canon Builder prompt explicitly performs an independent internal stress-test before Canon Freeze.

Do **not** inject a second ad hoc challenge block in n8n once the workflow is pinned to a GitHub commit containing that prompt. GitHub is the source of truth.

After Canon Builder succeeds, canon is frozen for the normal Draft Generation run.

There is no automatic CANON rescue loop before human review.

## 14. Casting Director

Keep separate in Phase 1.

Input:

- canon
- compact casting history

Use a capable creative model. It may use a cheaper model/settings than the main Developer, Canon Builder, Writer, or Forensic Editor when practical.

Avoid immediate lead repetition when an equally strong fresh choice exists.

## 15. Edition Architect

Keep.

Build `edition_plan` from canon and casting.

The plan must include `scene_ownership_plan`.

For film:

- THE MOVIE and THE SCENES should not fully stage the same substantial sequence

For television:

- THE SEASON = concrete macro movement
- THE EPISODES = selected specific stories
- 8 to 10 episode seasons usually spotlight roughly 4 to 6 episodes
- if THE FINISH owns finale pressure, do not fully spend it in THE EPISODES first

Do not merge Casting + Architect in Phase 1.

## 16. Gold calibration

Load approved Gold examples only for Edition Writer.

Do not send full Gold articles to Production Developer, Canon Builder, Casting Director, or Forensic Editor without a specific proven reason.

## 17. Edition Writer

Keep as a premium creative call.

Inputs only:

- canon
- casting
- edition plan
- relevant writing governance
- approved Gold examples

Output:

`draft_v1`

Do not pass discarded alternatives, Ideation scores, duplicate audits, broad catalog history, or social/visual guidance.

## 18. Deterministic pre-review diagnostics

Keep cheap diagnostics.

Record at minimum:

- blank draft warning
- approximate word count
- em dash presence
- backstage technology phrase warnings
- hard public-integrity leak matches
- required headings for resolved format
- paragraph count
- suspicious short-sentence / one-sentence-paragraph runs when practical
- suspicious long-sentence clusters when practical
- section-overlap diagnostic
- Cast completeness diagnostic when practical

For FILM compare THE MOVIE vs THE SCENES.

For SERIES / LIMITED_SERIES compare THE SEASON vs THE EPISODES.

These are heuristic signals for Forensic Editor, not automatic rewrite triggers.

## 19. Forensic Editor

Run one fresh cold-read review.

Inputs:

- canon
- edition plan
- exact `draft_v1`
- diagnostics

Output:

`editorial_review`

Require valid `overall_score` from 1.0 to 10.0.

The review may recommend:

- NONE
- PROSE
- EDITION
- CANON

During Draft Generation, `revision_route` is advisory metadata only.

Do not automatically execute the route.

Do not chase an 8.0 threshold.

## 20. No automatic revision or rescue

Remove/bypass from normal Draft Generation:

- Revision Writer
- EDITION rerun path
- CANON rerun path
- rescue cycle counting
- automatic second Architect/Writer/Editor loop

The human should see the first complete draft and its review before premium revision spend occurs.

Revision remains available later through explicit Publish Prep or force redevelopment.

## 21. No duplicate final Forensic call

Because normal Draft Generation does not revise article prose after Forensic Review, that review already belongs to the exact delivered article.

Reuse it.

Do not run another final Forensic Editor merely to certify identical text.

If an exceptional repair changes substantive public prose, the score becomes stale and the changed article must be reviewed before delivery.

Whitespace, delivery formatting, or other non-prose transformations do not require rescoring.

## 22. Deterministic final article QA

Keep.

Hard blockers:

- article blank/malformed
- final title blank
- missing valid numeric score
- invalid required structured objects
- impossible/missing required section structure
- unrepaired hard backstage/internal-process corruption that makes the artifact unusable

Warnings only:

- score below 8.0
- non-NONE recommended revision route
- mild overlap heuristic
- minor motif density
- minor rhythm/prose concerns
- small casting taste notes

A complete human-reviewable artifact should continue to delivery.

## 23. Remove social packaging from Draft Generation

Do not run:

- IG Asset Packet Builder
- IG Packet QA
- packet-only repair

`ig_packet_json` is not required for `DRAFTED`.

For new rows, leave it blank.

For forced redevelopment of an existing draft, preserve any prior valid IG packet.

Social packaging belongs to later Publish Prep after human approval.

## 24. Google Drive delivery

Destination:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE]`

Document content:

`NCS SCORE: X.X / 10`

blank line

final article only

Do not include internal JSON, review notes, diagnostics, prompts, or canon.

For force redevelopment, preserve the previous artifact until replacement succeeds.

## 25. Final Ideas update

Only after successful Drive persistence, update the same row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `status = DRAFTED`

Do not require or fabricate `ig_packet_json`.

Preserve an existing IG packet during forced Draft Generation unless a later Publish Prep flow intentionally replaces it.

`DRAFTED` is the last success-state write.

Never populate `published_url` or set `PUBLISHED`.

## 26. DRAFTED meaning

`DRAFTED` means:

- complete public article exists
- exact delivered article has a numeric Forensic score
- deterministic completion QA ran
- Drive persistence succeeded
- final title / URL / score were written successfully

It does not mean:

- social assets exist
- score >= 8.0
- review route = NONE
- no remaining notes
- automatically publication-ready

## 27. Execution summary

Return:

- source commit SHA
- idea ID
- resolved final title and format
- models/providers used
- whether research ran
- final review route
- holistic score
- final QA result
- section-overlap result
- Drive URL
- final status
- unresolved editorial warnings

Do not report an IG QA result because Draft Generation no longer runs IG packaging.

## 28. Publish Prep boundary

Publish Prep is a separate future/parallel workflow triggered only by explicit human selection.

Possible sequence:

`Load Draft + Review → optional targeted Revision Writer → optional final Forensic review → IG Packet Builder → IG QA → future image/social handoff`

Rules:

- PROSE revision may be run narrowly
- EDITION or CANON redevelopment requires explicit human action
- if public prose changes and an exact current score is still required, run a new Forensic review after the change
- do not automatically reopen development merely because the original score is in the 7s

## 29. Phase 1 non-goals

Do not:

- merge Production Developer + Canon Builder
- merge Casting + Architect yet
- remove Edition Architect
- merge Writer + Forensic Editor
- downgrade Edition Writer
- remove deterministic diagnostics
- add a new durable state table
- add a new humanization node
- add another automated quality loop

Test several productions before making further compression changes.

## 30. Migration note

This v2.3 spec intentionally supersedes older behavior that required `ig_packet_json` before `DRAFTED` and older behavior that automatically executed editorial revision routes.

The canonical contracts are:

- `Governance/ncs-generation-data-contract.md` v2.3+
- `Governance/ncs-data-contract.md` v1.5+
- `Governance/ncs-generation-review-revision-os.md` v2.6+

Existing DRAFTED rows with IG packets remain valid.
