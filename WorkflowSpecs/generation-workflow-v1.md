# Never Coming Soon
## n8n Generation Workflow Specification v2.1

> The filename is retained for compatibility with existing loaders. This document supersedes the old Productions-table architecture.

## 1. Objective

Build one clean n8n workflow that starts from one eligible row in the `Ideas` tab and ends with:

- a final Never Coming Soon article in Google Drive
- a valid holistic NCS score for that exact final article
- a validated `ig_packet_json` social handoff
- the same Ideas row set to `DRAFTED`

The workflow develops the imaginary production before it writes the public article.

It never publishes automatically.

## 2. Systems and state

- GitHub: source of truth for governance, prompts, schemas, Gold examples, and this specification
- Google Sheet `Never Coming Soon — Ideas`: catalog and durable lifecycle state
- Google Drive: finished human-readable draft store
- n8n execution memory: temporary Generation workspace
- Primary LLM: strongest practical OpenAI reasoning and creative model
- Independent critic model: strongest available independent critic when configured, otherwise clean-context primary-model fallback
- Web research: conditional and narrowly scoped
- n8n Code nodes: deterministic routing, diagnostics, JSON handling, QA, and state updates

There is no required Productions tab and no `production_id`.

## 3. Source snapshot

At the beginning of each live run, resolve the current commit SHA of GitHub `main` and store it as `source_commit_sha`.

Fetch every required runtime file using that exact commit SHA.

A single execution must not mix governance versions.

## 4. Runtime files

### Governance

Load:

- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-generation-review-revision-os.md`
- `Governance/ncs-story-development-standard.md`
- `Governance/ncs-relationship-story-standard.md`
- `Governance/ncs-research-grounding-standard.md`
- `Governance/ncs-casting-standard.md`
- `Governance/ncs-editorial-anatomy.md`
- `Governance/ncs-television-editorial-standard.md`
- `Governance/ncs-voice-constitution.md`
- `Governance/ncs-editorial-quality-standard.md`
- `Governance/ncs-publication-integrity-standard.md`
- `Governance/ncs-editorial-scoring-standard.md`
- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`
- `Governance/ncs-generation-data-contract.md`

### Prompts

Load all current files under `Prompts/Generation/`, including stages 01 through 10.

### Schemas

Load:

- `Schemas/production-development.schema.json`
- `Schemas/research-packet.schema.json`
- `Schemas/story-challenge.schema.json`
- `Schemas/canon-bible.schema.json`
- `Schemas/casting-plan.schema.json`
- `Schemas/edition-plan.schema.json`
- `Schemas/edition-draft.schema.json`
- `Schemas/editorial-review.schema.json`
- `Schemas/revision-output.schema.json`
- `Schemas/ig-asset-packet.schema.json`

### Approved writing calibration

Load:

- `Examples/Gold/film-01-the-tell.md`
- `Examples/Gold/film-02-clearance.md`

Do not automatically add completed NCS editions to Gold calibration.

Fail if required runtime files cannot be loaded. Do not silently substitute stale hard-coded prompt text.

## 5. Structured-output safety

Every OpenAI response schema must have a plain root object with `type = object` and `additionalProperties = false`.

At the root, do not use:

- `oneOf`
- `anyOf`
- `allOf`
- `enum`
- `const`
- `not`

Nested enums are allowed when supported.

Validate every response schema against the actual provider API before the first paid run after schema changes.

If model output is invalid JSON or schema-invalid, retry once using the original response and exact validation error for structure-only repair.

If repair fails, stop downstream stages.

## 6. Trigger and eligibility

Inputs:

- `idea_id`: optional string
- `force_redevelopment`: optional boolean, default false

If `idea_id` is supplied, find exactly that row.

If blank, select the first row from top to bottom where:

- `status = DEVELOPMENT_SELECT`
- `development_packet_json` is nonblank

Process exactly one idea per execution.

Normal eligibility:

- DEVELOPMENT_SELECT: eligible
- GENERATING: stop
- DRAFTED: require explicit force redevelopment
- PUBLISHED: stop

Force redevelopment:

- explicit `idea_id` required
- allow DEVELOPMENT_SELECT or DRAFTED
- preserve prior successful final fields and Drive artifact until replacement succeeds

Do not require the Ideation `format` cell to be populated.

## 7. Start lock and recovery memory

Before changing status, save:

`pre_generation_status = current row status`

Then set the exact row to:

`GENERATING`

Use `idea_id` as the row key.

Preserve `human_notes` and `published_url`.

Keep the full source row snapshot in execution memory.

If the run fails or fails final quality gates before successful delivery, restore the exact row to `pre_generation_status` only when its current status is still `GENERATING`.

For a new run this restores DEVELOPMENT_SELECT.

For a failed forced redevelopment of an existing successful draft this restores DRAFTED and preserves the prior final artifact fields.

Never reset an arbitrary GENERATING row.

## 8. Main creative sequence

### Node 1: Trigger / Input Normalize

Normalize inputs.

### Node 2: Resolve GitHub Source Commit

Pin `main` HEAD as `source_commit_sha`.

### Node 3: Load Runtime Files

Fetch all required files at the pinned SHA.

### Node 4: Select Ideas Row

Apply explicit or automatic selection rules.

### Node 5: Validate Eligibility

Validate lifecycle, packet existence, and force rules.

### Node 6: Save Pre-Generation State + Set GENERATING

Store `pre_generation_status`, then lock the exact row.

### Node 7: Build Source Bundle

Parse `development_packet_json` and assemble clean execution context.

### Node 8: Production Developer

Inputs: Development Packet + human notes.

Output: `production_development`.

For romance, romantic comedy, second-chance love, or a central two-person relationship, the Relationship Story Standard is active.

### Node 9: Research Needed?

Use nonempty `research_requests` as the decision signal.

### Node 10: Research Grounder, conditional

Research only requested questions.

### Node 11: Story Challenger

Use a fresh critic context.

Stress-test production quality, genre delivery, relationship quality when relevant, over-designedness, and format.

### Node 12: Canon Builder

Build `canon_bible` from packet, development, research, challenge, and human notes.

After success, canon is frozen unless one controlled CANON rescue is later authorized.

### Node 13: Casting Director

Build `casting_plan` from canon and compact actor-repeat memory when available.

### Node 14: Edition Architect

Build `edition_plan` from canon and casting.

The plan must include `scene_ownership_plan` from the current schema.

For film, allocate substantial sequences so THE MOVIE and THE SCENES do not fully stage the same event.

For television, allocate substantial sequences so THE SEASON and THE EPISODES do not fully stage the same event.

### Node 15: Load Gold Calibration

Provide both approved Gold film examples to writing and revision stages.

### Node 16: Edition Writer

Inputs only:

- canon bible
- casting plan
- edition plan
- Gold examples

Output: `draft_v1`.

Do not pass discarded development alternatives, Ideation scores, or catalog history.

## 9. Pre-review diagnostics

### Node 17: Deterministic Pre-Review Diagnostics

Record at minimum:

- blank draft warning
- approximate word count
- em dash presence
- backstage technology phrase warnings
- hard public-integrity leak matches
- required-heading presence for resolved format
- total paragraph count
- non-dialogue one-sentence paragraph count
- longest consecutive run of non-dialogue one-sentence paragraphs
- locations of suspicious runs when practical
- section-overlap diagnostic

### Section-overlap diagnostic

This is heuristic evidence, not an automatic rewrite.

For FILM:

- extract THE MOVIE and THE SCENES
- compare normalized phrases, distinctive event language, repeated dialogue, and named sequence details
- flag likely substantial overlap when the same event appears to receive close treatment in both

For SERIES and LIMITED_SERIES:

- compare THE SEASON and THE EPISODES the same way

Pass overlap findings to the Forensic Editor as diagnostics.

Hard public-integrity leak matches are material problems.

## 10. Review and revision

### Node 18: Forensic Editor

Inputs:

- canon bible
- edition plan
- exact current draft
- deterministic diagnostics

Output: `editorial_review`.

Required fields include:

- `overall_score`
- `genre_specific_assessment`
- `scene_overlap_flags`

The score belongs only to the exact draft reviewed.

A score below 8.0 may not use `revision_route = NONE`.

### Node 19: Route by revision_route

Allowed routes:

- NONE
- PROSE
- EDITION
- CANON

Track `rescue_cycle_count`.

Maximum deep rescue cycles: 1.

### PROSE

Run Revision Writer once using latest canon, edition plan, current draft, review, and Gold examples.

### EDITION

If deep rescue remains available:

1. increment rescue count
2. rerun Edition Architect with review context
3. rerun Edition Writer
4. rerun diagnostics
5. rerun Forensic Editor
6. route once more

### CANON

If deep rescue remains available:

1. increment rescue count
2. rerun Canon Builder with review context
3. regenerate casting
4. regenerate edition plan
5. rerun Edition Writer
6. rerun diagnostics
7. rerun Forensic Editor
8. route once more

Do not create endless loops.

## 11. Final Forensic Review Gate

This gate applies to the exact final-text candidate.

If the final-text candidate is byte-for-byte identical to the draft in the most recent review, that review may be reused.

Otherwise run Forensic Editor again.

Require:

- numeric `overall_score`
- `overall_score >= 8.0`
- `revision_route = NONE`

Never substitute `N/A`.

Never attach an earlier score to revised text.

If the final draft remains below 8.0 or still requests revision after the allowed automated path, stop before Drive delivery and restore `pre_generation_status`.

Surface the unresolved issues rather than pretending success.

## 12. Deterministic Final Article QA

### Node 22: Final QA

Hard-gate checks:

- final article nonblank
- final title nonblank
- exact final review exists
- numeric `overall_score >= 8.0`
- final `revision_route = NONE`
- no em dash character
- no obvious backstage technology references
- no hard internal editorial leakage
- required headings for resolved format
- plausible word count
- valid final JSON objects

Additional diagnostics:

- major-name consistency when practical
- episode-count consistency when practical
- paragraph-rhythm metrics
- section-overlap warning state

A section-overlap warning is passed to review and summary. If the final Forensic Editor accepted the structure with `revision_route = NONE`, the heuristic alone does not hard fail delivery.

If any hard gate fails, stop before Drive and restore `pre_generation_status`.

## 13. IG Asset Packet Builder

### Node 23: IG Asset Packet Builder

Inputs:

- final canon
- final title
- exact final article
- final overall score
- resolved canonical format

Output: `ig_packet_json`.

The packet must contain:

- `version = ncs_ig_v1`
- final title
- exact uppercase format enum
- genre
- campaign brief
- logo treatment
- exact short social caption
- locked Slide 1 packet
- locked Slide 2 packet
- locked Slide 3 packet

## 14. IG Packet Deterministic QA

### Node 24: IG Packet QA

Validate the exact final object that will be persisted, after any repair, normalization, mapping, or transformation.

Require:

- schema-valid packet against the current pinned `Schemas/ig-asset-packet.schema.json`
- `version = ncs_ig_v1`
- `format` exactly equals resolved canon format and is one of FILM, SERIES, LIMITED_SERIES
- final title matches article title
- exactly three slide objects
- Slide 1 type = cover_poster
- Slide 1 `never_coming_soon_presents = Never Coming Soon presents`
- Slide 2 type = premise
- Slide 3 type = ncs_close
- Slide 2 header is not a generic label such as The Premise, About the Movie, or The Story
- Slide 2 body copy nonblank
- Slide 3 newsletter line exactly = THE FULL STORY IN NEVER COMING SOON
- Slide 3 CTA exactly = LINK IN BIO
- Hollywood line nonblank and not identical to Slide 1 tagline
- caption nonblank and not identical to Slide 2 body copy
- no em dash character in public packet copy
- no backstage technology language
- no hard internal editorial terminology
- billing block contains no fake production-process credit or unnecessary participation claim

Billing block deny examples include:

- Filmed in
- Shot on location
- Poster design by
- Poster design and finishing by the NCS campaign team
- Campaign by
- Artwork by the NCS team
- Generated by
- Created with

If packet QA fails, allow one packet-only repair.

After repair, validate the exact repaired object again.

Do not report QA success based on a pre-repair object.

Do not rewrite the article because the packet alone failed.

If the final packet still fails, stop before Drive and do not set DRAFTED.

## 15. Google Drive delivery

### Node 25: Google Drive Delivery

Destination:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE]`

Google Doc contents:

`NCS SCORE: X.X / 10`

blank line

final article only

Do not include internal JSON, review notes, diagnostics, prompts, or canon.

For force redevelopment, preserve the previous artifact until replacement succeeds.

## 16. Final Ideas update

### Node 26: Final Ideas Row Update

Only after successful Drive persistence, update the same row:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` must be the last success-state write.

Never populate `published_url`.

Never set `PUBLISHED`.

## 17. Execution summary

Return:

- source commit SHA
- idea ID
- resolved final title and format
- models/providers used
- whether research ran
- rescue route if any
- final review route
- final holistic score
- final QA result
- section-overlap diagnostic result
- IG packet QA result
- Drive URL
- final status
- unresolved warnings, if any

## 18. Model roles

Use the highest-capability practical models for core creative work.

- Production Developer: strongest OpenAI reasoning and creative model
- Research Grounder: strong web-grounded factual model
- Story Challenger: strongest configured independent critic, otherwise fresh strongest OpenAI
- Canon Builder: strongest OpenAI reasoning model
- Casting Director: strongest OpenAI model with creative range
- Edition Architect: strongest OpenAI reasoning model
- Edition Writer: strongest OpenAI writing model
- Forensic Editor: strongest independent critic when configured, otherwise fresh strongest OpenAI
- Revision Writer: strongest OpenAI writing model with controlled creativity
- IG Asset Packet Builder: strong structured-output model with visual campaign judgment

## 19. Context hygiene

Production Developer sees Development Packet + human notes.

Research Grounder sees production context + explicit research requests.

Story Challenger sees packet + development + research + compact repetition memory.

Canon Builder sees packet + development + research + challenge + human notes.

Casting Director sees canon + casting-history summary.

Edition Architect sees canon + casting.

Edition Writer sees canon + casting + edition plan + Gold examples.

Forensic Editor sees canon + edition plan + exact draft + diagnostics.

Revision Writer sees latest canon + latest edition plan + current draft + review + Gold examples.

IG Asset Packet Builder sees final canon + final article + final score + resolved format + visual/social governance.

This separation is mandatory.

## 20. Public-integrity hard-leak list

At minimum, scan public article and social copy case-insensitively for:

- contained proof
- extractable play:
- extractable scene:
- results stay protected
- unresolved value
- genre proof
- spoiler protection
- reveal policy
- canon bible
- canon freeze
- development packet
- story challenge
- edition plan
- revision route
- proof of existence
- evidence of spectatorship
- engine demonstration

Semantic leakage beyond this exact list remains the Forensic Editor's responsibility.

## 21. Error recovery workflow

Use a small companion Error Trigger workflow.

It must:

1. recover the exact failed `idea_id`
2. find that exact Ideas row
3. act only when current status is GENERATING
4. recover `pre_generation_status` from failed execution context
5. restore the exact row to that prior status
6. modify no unrelated fields

If prior status was DRAFTED, preserve all prior successful final fields.

If exact `idea_id` or prior status cannot be recovered reliably, do not reset an arbitrary row.

The recovery workflow must be idempotent.

## 22. Persistence safety

- update Ideas rows by `idea_id`, not visible row number
- preserve `idea_id`, `created_at`, `human_notes`, and `published_url`
- do not persist intermediate creative stages to Sheets
- do not regenerate creative content merely because a Sheet or Drive write failed
- retry transient writes with backoff
- parse JSON once and stringify once
- validate the exact final structured objects after all transformations
- preserve prior successful final fields during a failed force-redevelopment attempt

## 23. Non-goals

Do not add unless later requested:

- separate Productions state table
- `production_id`
- durable checkpoint/resume state for each creative stage
- automatic publication
- automated newsletter sending
- multi-row character database
- endless self-revision loops
- extra social slides beyond the locked three-slide packet

## 24. Quality principle

Creative sophistication belongs in the production-development and editorial system.

Operational state should remain boring.

The workflow should be easy to understand, easy to debug, and difficult to leave in a false-success state.
