# Never Coming Soon
## n8n Generation Workflow Specification v2.0

> The filename is retained for compatibility with existing loaders. This document supersedes the old Productions-table v1 architecture.

## 1. Objective

Build one clean n8n workflow that starts from one eligible row in the `Ideas` tab and ends with:

- a final Never Coming Soon article in Google Drive
- a valid holistic NCS score for that exact final article
- a validated `ig_packet_json` social handoff
- the same Ideas row set to `DRAFTED`

The workflow develops the imaginary production before it writes the public article.

It never publishes automatically.

## 2. Systems

- GitHub: source of truth for governance, prompts, schemas, Gold examples, and this specification
- Google Sheet `Never Coming Soon — Ideas`: catalog and durable lifecycle state
- Google Drive: finished human-readable draft store
- n8n execution memory: temporary Generation workspace
- Primary LLM: strongest practical OpenAI reasoning/creative model
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

Load all current files under `Prompts/Generation/`, including:

- `01-production-developer.*`
- `02-research-grounding.*`
- `03-story-challenger.*`
- `04-canon-builder.*`
- `05-casting-director.*`
- `06-edition-architect.*`
- `07-edition-writer.*`
- `08-forensic-editor.*`
- `09-revision-writer.*`
- `10-ig-asset-packet-builder.*`

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

Fail the run if required runtime files cannot be loaded.

Do not silently substitute stale hard-coded prompt text.

## 5. Structured-output safety

Every OpenAI response schema must have a plain root object:

```json
{
  "type": "object",
  "properties": {},
  "required": [],
  "additionalProperties": false
}
```

At the root, do not use:

- `oneOf`
- `anyOf`
- `allOf`
- `enum`
- `const`
- `not`

Nested property enums and nullable nested fields may be used when supported.

Do not create separate root schemas for film, series, and limited series.

Format-specific behavior belongs in prompts and ordinary fields.

Validate all schemas against the actual provider API before the first paid live run after schema changes.

## 6. Trigger contract

Support:

- `idea_id`: optional string
- `force_redevelopment`: optional boolean, default false

If `idea_id` is supplied:

- find exactly that row
- fail clearly if not found

If `idea_id` is blank:

- select the first row from top to bottom where `status = DEVELOPMENT_SELECT` and `development_packet_json` is nonblank

Process exactly one idea per execution.

## 7. Eligibility

When `force_redevelopment = false`:

- `DEVELOPMENT_SELECT`: eligible
- `GENERATING`: stop to prevent duplicate work
- `DRAFTED`: stop and require explicit force redevelopment
- `PUBLISHED`: stop
- all earlier Ideation statuses: not eligible

When `force_redevelopment = true`:

- explicit `idea_id` is required
- allow eligible `DEVELOPMENT_SELECT` or `DRAFTED`
- preserve previous successful final artifacts until replacement succeeds
- do not auto-redevelop `PUBLISHED`

Do not require the Ideation `format` cell to be populated. Generation may resolve format from the packet and its own development judgment.

## 8. Start lock

After selection and eligibility validation, set the exact Ideas row to:

`GENERATING`

Use `idea_id` to target the row.

Do not modify `human_notes` or `published_url`.

Keep a snapshot of the selected row in execution memory as `source_idea`.

Safely parse `development_packet_json`.

If parsing fails, stop and restore the row to `DEVELOPMENT_SELECT`.

## 9. Main node sequence

### Node 1: Trigger / Input Normalize

Normalize `idea_id` and `force_redevelopment`.

### Node 2: Resolve GitHub Source Commit

Resolve `main` HEAD and store `source_commit_sha`.

### Node 3: Load Runtime Files

Fetch all required files at the pinned SHA.

### Node 4: Select Ideas Row

Apply explicit or blank-ID selection rules.

### Node 5: Validate Eligibility

Validate lifecycle, packet existence, and force-redevelopment rules.

### Node 6: Set GENERATING

Lock the exact row.

### Node 7: Build Source Bundle

Create the clean execution object containing:

- source row
- parsed Development Packet
- human notes
- pinned governance
- prompts
- schemas
- Gold examples

### Node 8: Production Developer

Inputs:

- Development Packet
- human notes

Output:

`production_development`

Do not persist to Sheets.

### Node 9: Research Needed?

Use nonempty `research_requests` as the decision signal.

### Node 10: Research Grounder, conditional

Research only requested questions.

Return compact factual grounding.

Do not force research findings into the story.

### Node 11: Story Challenger

Use a fresh critic context.

Inputs:

- Development Packet
- production development
- research if any
- compact catalog repetition memory only when already cleanly available

Output:

`story_challenge`

### Node 12: Canon Builder

Inputs:

- Development Packet
- production development
- research if any
- story challenge
- human notes

Output:

`canon_bible`

After success, canon is frozen unless one controlled CANON rescue is later authorized.

### Node 13: Casting Director

Inputs:

- canon bible
- compact actor-repeat memory when available

Output:

`casting_plan`

### Node 14: Edition Architect

Inputs:

- canon bible
- casting plan

Output:

`edition_plan`

For television, load and apply the dedicated Television Editorial Standard.

### Node 15: Load Gold Calibration

Provide both approved Gold film examples to writing/revision stages as craft calibration.

Gold examples are not story templates.

### Node 16: Edition Writer

Clean inputs only:

- canon bible
- casting plan
- edition plan
- approved Gold examples

Output:

`draft_v1`

Do not pass Challenger notes, discarded alternatives, Ideation scores, or catalog history.

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

Paragraph metrics are diagnostic.

Hard public-integrity leak matches should be treated as material problems.

### Node 18: Forensic Editor

Inputs:

- canon bible
- edition plan
- current draft
- deterministic diagnostics

Output:

`editorial_review`

Required field:

`overall_score`

The score belongs only to the exact draft reviewed.

### Node 19: Route by revision_route

Allowed routes:

- `NONE`
- `PROSE`
- `EDITION`
- `CANON`

Track `rescue_cycle_count`.

Maximum deep rescue cycles: 1.

### Node 20A: NONE

Use the current draft as final-text candidate.

### Node 20B: PROSE

Run Revision Writer once using:

- latest canon
- latest edition plan
- current draft
- review
- Gold examples

Output becomes final-text candidate.

### Node 20C: EDITION

If deep rescue remains available:

1. increment rescue count
2. rerun Edition Architect with review context
3. rerun Edition Writer
4. rerun pre-review diagnostics
5. rerun Forensic Editor
6. route once more

Do not create endless loops.

### Node 20D: CANON

If deep rescue remains available:

1. increment rescue count
2. rerun Canon Builder with review context
3. regenerate casting
4. regenerate edition plan
5. rerun Edition Writer
6. rerun pre-review diagnostics
7. rerun Forensic Editor
8. route once more

Do not create endless loops.

### Node 21: Final Forensic Review Gate

This node is mandatory whenever article text changed after the most recent review.

If the current final-text candidate is byte-for-byte the same article reviewed by Node 18 or a later review, reuse that review.

Otherwise run Forensic Editor one final time on the exact final-text candidate.

The review returned here is `final_editorial_review`.

Require:

- numeric `overall_score`
- 1.0 <= score <= 10.0

Never substitute `N/A`.

Never attach an earlier-draft score to a revised article.

### Node 22: Deterministic Final QA

Hard-gate checks:

- final article nonblank
- final title nonblank
- valid numeric final `overall_score`
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

If a hard gate fails, stop before Drive and do not set `DRAFTED`.

### Node 23: IG Asset Packet Builder

Inputs:

- final canon
- final title
- exact final article
- final overall score

Use:

- `Prompts/Generation/10-ig-asset-packet-builder.system.md`
- `Prompts/Generation/10-ig-asset-packet-builder.user.md`
- `Schemas/ig-asset-packet.schema.json`

Output:

`ig_packet_json`

The packet contains:

- `version = ncs_ig_v1`
- final title
- final format
- genre
- campaign brief
- logo treatment
- exact short social caption
- locked Slide 1 packet
- locked Slide 2 packet
- locked Slide 3 packet

### Node 24: IG Packet Deterministic QA

Require:

- schema-valid packet
- final title matches article title
- exactly three slide objects
- Slide 1 type `cover_poster`
- Slide 2 type `premise`
- Slide 3 type `ncs_close`
- Slide 2 header is not a generic label such as `The Premise`, `About the Movie`, or `The Story`
- Slide 2 body copy nonblank
- Slide 3 newsletter line exactly `THE FULL STORY IN NEVER COMING SOON`
- Slide 3 CTA exactly `LINK IN BIO`
- Hollywood line nonblank and not identical to Slide 1 tagline
- caption nonblank
- no em dash character anywhere in public packet copy
- no backstage technology language
- no hard internal editorial terminology

If packet QA fails, allow one structure/copy repair of the packet only.

Do not rewrite the article merely because the asset packet failed.

### Node 25: Google Drive Delivery

Destination:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE]`

Google Doc contents:

`NCS SCORE: X.X / 10`

blank line

final article only

Do not include internal JSON, review notes, diagnostics, prompts, or canon.

For force redevelopment, preserve the previous artifact until replacement succeeds.

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

### Node 27: Return Execution Summary

Return a concise operator summary including:

- source commit SHA
- idea ID
- resolved final title and format
- models/providers used
- whether research ran
- rescue route if any
- final review route
- final holistic score
- final QA result
- IG packet QA result
- Drive URL
- final status
- unresolved warnings, if any

## 10. Model roles

Use the highest-capability practical models, not cheaper models by default for core creative work.

Recommended behavior:

- Production Developer: strongest OpenAI reasoning/creative model
- Research Grounder: strong model with web search and factual discipline
- Story Challenger: strongest configured independent critic, otherwise clean-context strongest OpenAI
- Canon Builder: strongest OpenAI reasoning model
- Casting Director: strongest OpenAI model with creative range
- Edition Architect: strongest OpenAI reasoning model
- Edition Writer: strongest OpenAI writing model
- Forensic Editor: strongest independent critic when configured, otherwise clean-context strongest OpenAI
- Revision Writer: strongest OpenAI writing model with controlled creativity
- IG Asset Packet Builder: strong model with visual campaign judgment and disciplined structured output; it does not need to be the most expensive reasoning model if quality is validated

## 11. Context hygiene

Production Developer sees Development Packet + human notes.

Research Grounder sees production context + explicit research requests.

Story Challenger sees packet + development + research + compact repetition memory.

Canon Builder sees packet + development + research + challenge + human notes.

Casting Director sees canon + casting-history summary.

Edition Architect sees canon + casting.

Edition Writer sees canon + casting + edition plan + Gold examples.

Forensic Editor sees canon + edition plan + exact draft + deterministic diagnostics.

Revision Writer sees latest canon + latest edition plan + current draft + review + Gold examples.

IG Asset Packet Builder sees final canon + final article + final score + visual/social governance.

This separation is mandatory.

## 12. Public-integrity hard-leak list

At minimum, scan public article and public social copy case-insensitively for:

- `contained proof`
- `extractable play:`
- `extractable scene:`
- `results stay protected`
- `unresolved value`
- `genre proof`
- `spoiler protection`
- `reveal policy`
- `canon bible`
- `canon freeze`
- `development packet`
- `story challenge`
- `edition plan`
- `revision route`
- `proof of existence`
- `evidence of spectatorship`
- `engine demonstration`

Semantic leakage beyond this exact list remains the Forensic Editor's responsibility.

## 13. Failure recovery workflow

Use a small companion workflow named clearly, for example:

`NCS - Generation Error Recovery v2.0`

Assign it through n8n `settings.errorWorkflow`.

The companion should:

1. receive the failed execution context
2. recover the exact failed `idea_id`
3. find that exact Ideas row
4. only act if the row is still `GENERATING`
5. reset `GENERATING` -> `DEVELOPMENT_SELECT`
6. modify no other fields

If the exact `idea_id` cannot be recovered reliably, do not reset an arbitrary GENERATING row.

The recovery workflow must be idempotent.

If the row is already `DRAFTED`, `PUBLISHED`, or otherwise no longer `GENERATING`, do nothing.

## 14. Structured-output repair

If model output is invalid JSON or schema-invalid, retry once with the same model using the original response and exact validation error, instructing structure-only repair unless required data is genuinely missing.

If repair fails, stop downstream stages that would corrupt state.

Do not solve schema errors by adding alternate persistence paths.

## 15. Persistence safety

- update Ideas rows by `idea_id`, not visible row number
- preserve `idea_id`, `created_at`, `human_notes`, and `published_url`
- do not persist intermediate creative stages to Sheets
- do not regenerate creative content merely because a Sheet or Drive write failed
- retry transient writes with backoff
- parse JSON once and stringify once
- preserve prior successful final fields during a failed force-redevelopment attempt

## 16. No hidden publication decision

Generation ends at `DRAFTED`.

A later human or explicit publishing workflow may set:

`DRAFTED` -> `PUBLISHED`

Generation never makes that decision.

## 17. Non-goals

Do not add unless later requested:

- a separate Productions state table
- `production_id`
- durable checkpoint/resume state for each creative stage
- automatic publication
- automated newsletter sending
- multi-row character database
- endless self-revision loops
- extra social slides beyond the locked three-slide canonical packet

## 18. Quality principle

Creative sophistication belongs in the production-development and editorial system.

Operational state should remain boring.

The workflow should be easy to understand, easy to debug, and difficult to leave in a false-success state.
