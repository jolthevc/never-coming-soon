# Never Coming Soon
## n8n Generation Workflow Specification v2.4

> Filename retained for loader compatibility.

## 1. Objective

Build one lean Generation workflow that starts from one eligible Ideas row and ends with:

- a complete Never Coming Soon article in Google Drive
- a valid holistic NCS score for that exact delivered article
- a schema-valid `ig_packet_json` for downstream media asset generation
- the same Ideas row set to `DRAFTED`

Generation develops the production, writes the article, cold-reviews it once, creates the media/social handoff, and delivers the complete package for human judgment.

It does **not** automatically execute editorial revision routes or deep rescue loops.

## 2. Core principle

Spend premium model calls where they create creative quality or a required downstream artifact.

Normal paid-model path:

1. Production Developer
2. Research Grounder only when genuinely requested
3. Canon Builder with internal independent challenge
4. Casting Director
5. Edition Architect
6. Edition Writer
7. Forensic Editor
8. IG Asset Packet Builder

With no research, this is seven model calls.

Do not add another prose/humanization node.

## 3. Systems and state

- GitHub: source of truth for governance, prompts, schemas, Gold examples, and this spec
- Google Sheet `Never Coming Soon — Ideas`: catalog and durable lifecycle state
- Google Drive: finished human-readable draft store
- n8n execution memory: temporary workspace
- n8n Code nodes: deterministic routing, diagnostics, JSON handling, QA, and state updates

There is no required Productions tab and no `production_id`.

Generation does not write an intermediate `GENERATING` status.

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

### IG Asset Packet Builder

Use:

- final canon
- final title
- exact final article
- final holistic score
- resolved canonical format
- visual constitution
- social asset standard
- stage 10 prompt
- IG asset packet schema

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

A shared schema-sanitization choke point in orchestration is acceptable so long as it preserves the intended required fields and nested validation semantics.

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
- DRAFTED: require explicit force redevelopment
- PUBLISHED: stop

Force redevelopment requires explicit `idea_id` and may run on DEVELOPMENT_SELECT or DRAFTED.

Do not require the source `format` cell to be populated.

## 8. No intermediate lifecycle lock

Do not write `GENERATING` at run start.

For a normal run, the selected row remains `DEVELOPMENT_SELECT` throughout execution.

For forced redevelopment, the selected row remains `DRAFTED` throughout execution and prior successful final fields remain untouched.

Do not write partial final fields during the run.

If the run fails, no lifecycle recovery write is needed because no intermediate lifecycle mutation occurred.

If duplicate-run protection is needed, use n8n execution-level controls, explicit `idea_id` discipline, or other orchestration safeguards rather than another durable Sheet status.

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

Do not call stage 03 or require `Schemas/story-challenge.schema.json` in normal Generation.

The files may remain in GitHub for history/future experiments.

Quality protection moves into the Canon Builder prompt.

## 13. Canon Builder with internal challenge

Build `canon_bible` from:

- Development Packet
- Production Developer output
- research packet if any
- human notes

The current Canon Builder prompt explicitly performs an independent internal stress-test before Canon Freeze.

Do **not** inject a second ad hoc challenge block in n8n once the workflow is pinned to a GitHub commit containing that prompt.

After Canon Builder succeeds, canon is frozen for the normal Generation run.

There is no automatic CANON rescue loop before human review.

## 14. Casting Director

Keep separate in Phase 1.

Input:

- canon
- compact casting history

Use a capable creative model. It may use cheaper model/settings than the main Developer, Canon Builder, Writer, or Forensic Editor when practical.

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

During normal Generation, `revision_route` is advisory metadata only.

Do not automatically execute the route.

Do not chase an 8.0 threshold.

## 20. No automatic revision or rescue

Remove/bypass from normal Generation:

- Revision Writer
- EDITION rerun path
- CANON rerun path
- rescue cycle counting
- automatic second Architect/Writer/Editor loop

The human should see the first complete draft and its review before premium revision spend occurs.

Revision remains available later through explicit human action.

## 21. No duplicate final Forensic call

Because normal Generation does not revise article prose after Forensic Review, that review already belongs to the exact delivered article.

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

A complete human-reviewable article should continue to IG packaging.

## 23. IG Asset Packet Builder

Keep this stage in normal Generation.

Inputs:

- final canon
- final title
- exact final article
- final overall score
- resolved canonical format

Output:

`ig_packet_json`

Require:

- `version = ncs_ig_v1`
- final title
- exact uppercase format enum
- genre
- campaign brief
- logo treatment
- short social caption
- locked Slide 1 packet
- locked Slide 2 packet
- locked Slide 3 packet

Campaign coherence should not become motif repetition. One signature visual motif should normally appear explicitly on no more than two slides. Slide 3 may inherit campaign identity through palette, typography, or texture alone.

## 24. IG Packet QA

Validate the exact final object that will be persisted after any repair, normalization, mapping, or transformation.

Require:

- schema-valid packet against current `Schemas/ig-asset-packet.schema.json`
- `version = ncs_ig_v1`
- `format` exactly equals resolved canon format and is one of FILM, SERIES, LIMITED_SERIES
- final title matches article title
- exactly three slide objects
- Slide 1 type = cover_poster
- Slide 1 `never_coming_soon_presents = Never Coming Soon presents`
- Slide 2 type = premise
- Slide 3 type = ncs_close
- Slide 2 header is not a generic label such as The Premise, About the Movie, The Story, or Synopsis
- Slide 2 body copy nonblank
- Slide 3 newsletter line exactly = THE FULL STORY IN NEVER COMING SOON
- Slide 3 CTA exactly = LINK IN BIO
- Hollywood line nonblank and not identical to Slide 1 tagline
- caption nonblank and not identical to Slide 2 body copy
- no em dash character in public packet copy
- no backstage technology language
- no hard internal editorial terminology
- billing block contains no fake production-process credit or unnecessary participation claim

If packet QA fails, allow one packet-only repair and validate the repaired object again.

Do not rewrite the article because the packet alone failed.

If the final packet remains structurally invalid, stop before `DRAFTED` because the downstream media handoff is incomplete.

## 25. Google Drive delivery

After article QA and IG Packet QA succeed, write:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE]`

Document content:

`NCS SCORE: X.X / 10`

blank line

final article only

Do not include internal JSON, review notes, diagnostics, prompts, or canon.

For force redevelopment, preserve the previous artifact until the replacement package succeeds.

## 26. Final Ideas update

Only after successful IG validation and Drive persistence, update the same row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` is the only lifecycle status written by a successful Generation run.

Never populate `published_url` or set `PUBLISHED`.

## 27. DRAFTED meaning

`DRAFTED` means:

- complete public article exists
- exact delivered article has a numeric Forensic score
- deterministic article completion QA ran
- schema-valid final IG packet exists
- Drive persistence succeeded
- final title / URL / score / IG packet were written successfully

It does not mean:

- score >= 8.0
- review route = NONE
- no remaining notes
- automatically publication-ready

## 28. Execution summary

Return:

- source commit SHA
- idea ID
- resolved final title and format
- models/providers used
- whether research ran
- final review route
- holistic score
- final article QA result
- section-overlap result
- IG packet QA result
- Drive URL
- final status
- unresolved editorial warnings

## 29. Later revision boundary

Later explicit human-selected work may run:

`Load Draft + Review → optional targeted Revision Writer → optional final Forensic review → regenerate IG packet if article/title/canon materially changed → IG QA → downstream media/publishing handoff`

Rules:

- PROSE revision may be run narrowly
- EDITION or CANON redevelopment requires explicit human action
- if public prose changes and an exact current score is required, run a new Forensic review after the change
- if public prose, title, canon, or campaign direction changes materially, regenerate `ig_packet_json`
- do not automatically reopen development merely because the original score is in the 7s

## 30. Phase 1 non-goals

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

## 31. Migration note

This v2.4 spec supersedes both:

- older behavior that used an intermediate `GENERATING` Sheet status
- the brief draft-first experiment that deferred IG packaging until later

The current architecture intentionally keeps IG packaging in normal Generation because downstream media asset generation depends on `ig_packet_json`.

The canonical contracts are:

- `Governance/ncs-generation-data-contract.md` v2.4+
- `Governance/ncs-data-contract.md` v1.6+
- `Governance/ncs-generation-review-revision-os.md` v2.7+
