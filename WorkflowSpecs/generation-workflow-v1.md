# Never Coming Soon
## n8n Generation Workflow Specification v3.0

> Filename retained for loader compatibility.

## 1. Objective

Build one lean Generation workflow that turns one Development-Selected idea into:

- a complete frozen internal production canon
- a durable human-readable production treatment in Google Drive
- a schema-valid six-slide `ig_packet_json`
- the same Ideas row set to `DRAFTED`

The social release packet is the required consumer-facing handoff.

A long-form article, casting pass, editorial score, and forensic review are not required in the normal path.

## 2. Core principle

**Develop the production once. Package it once. Do not spend model calls polishing an article unless a human actually wants the article.**

Normal paid-model path:

1. Production Builder
2. Social Release Builder

Optional paid-model work:
- Research Grounder only when explicitly requested or clearly required
- one Production Builder rerun after research when the research materially changes canon
- long-form writing only through a later explicit human action

## 3. Systems and state

- GitHub: source of truth for governance, prompts, schemas, and workflow specs
- Google Sheet `Never Coming Soon — Ideas`: catalog and durable lifecycle state
- Google Drive: durable production treatment store
- n8n execution memory: temporary workspace

The Sheet remains one row per concept.

## 4. Source snapshot

At the beginning of every live run:

1. resolve current GitHub `main` commit SHA
2. store it as `source_commit_sha`
3. load all prompt, governance, and schema files from that same snapshot

Do not mix source versions inside one execution.

## 5. Trigger and eligibility

Inputs:

- `idea_id`: optional
- `force_redevelopment`: optional boolean, default false
- `research_questions`: optional array
- `human_campaign_notes`: optional string

Normal eligibility:

- row status = `DEVELOPMENT_SELECT`
- `development_packet_json` is nonblank

Forced redevelopment may explicitly target an existing `DRAFTED` row.

Do not clear prior successful output before the replacement run succeeds.

## 6. Build source bundle

Parse:
- `development_packet_json`
- `human_notes`
- any explicit `research_questions`

Do not load old draft articles, old review scores, social packets, duplicate audits, broad catalog history, or discarded alternatives unless the run explicitly needs them.

## 7. Production Builder

Use:

- `Prompts/Generation/01-production-builder.system.md`
- `Prompts/Generation/01-production-builder.user.md`
- `Schemas/canon-bible.schema.json`
- relevant brand, story, relationship, television, research, and integrity governance

Input:
- Development Packet
- optional research packet
- optional human notes

Output:
- `canon_bible`

This call owns:
- final title
- final format
- genre
- logline
- world
- characters
- central relationships
- complete story
- actual ending
- signature scenes
- genre delivery
- public unresolved value
- public genre demonstrations
- continuity facts

No article is written here.

No real actors are cast here.

No social copy is written here.

## 8. Optional research

Research is not part of the default path.

Run Research Grounder only when:
- the human explicitly supplies research questions, or
- an orchestration rule has a strong reason to require factual grounding before canon can be trusted

If research materially changes the production:
1. run Research Grounder
2. rerun Production Builder once with the research packet
3. freeze the second canon

Do not build open-ended research loops.

## 9. Deterministic production treatment

After canon is frozen, create one durable human-readable treatment from the Canon Bible without another creative-model call.

The treatment should include, in a clean readable order:

- final title
- format and genre
- logline
- core promise
- creative kernel
- world
- featured characters
- central relationships
- complete internal story
- signature scenes
- genre delivery
- series engine and season-one material when relevant
- continuity facts

This is an internal source artifact, not a public article.

Persist it to:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE] - Production Treatment`

The legacy Sheet field `draft_url` may continue to point to this treatment for compatibility.

Do not spend an LLM call beautifying it.

## 10. Social Release Builder

Use:

- final Canon Bible
- optional human campaign notes
- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`
- `Governance/ncs-publication-integrity-standard.md`
- `Governance/ncs-voice-constitution.md`
- `Prompts/Generation/10-ig-asset-packet-builder.system.md`
- `Prompts/Generation/10-ig-asset-packet-builder.user.md`
- `Schemas/ig-asset-packet.schema.json`

Output:

`ig_packet_json`

The packet is the exact downstream asset-generation contract.

Do not pass article drafts or editorial scores because they are not required.

## 11. IG Packet v2 requirements

Require:

- `version = ncs_ig_v2`
- final title equals canon title
- format equals canon format
- FILM -> MOVIE IDEA
- SERIES / LIMITED_SERIES -> SHOW IDEA
- exactly six slides
- Slide 1 type = `hook`
- Slide 2 type = `premise`
- Slide 3 type = `characters`
- Slide 4 type = `movie_texture`
- Slide 5 type = `poster`
- Slide 6 type = `ncs_close`
- Slide 3 contains 2 to 4 featured characters
- Slide 6 slogan exactly matches schema
- caption nonblank
- visual continuity object nonblank
- every required image prompt nonblank
- no em dash character in public copy
- no backstage technology language
- no false actor/studio/crew participation claims

The packet should be validated after any normalization or repair and immediately before persistence.

Allow one packet-only repair if schema or hard integrity QA fails.

Do not rerun Production Builder merely because packet formatting failed.

## 12. Image workflow boundary

The downstream image workflow should consume `ig_packet_json` verbatim.

It should not decide:
- slide order
- copy
- featured characters
- reveal strategy
- CTA
- poster tagline
- character count
- story emphasis

The downstream image model executes visual direction only.

Exact text and logo placement should be deterministic.

## 13. Final Ideas update

Only after:
- Canon Bible is valid
- treatment persistence succeeds
- IG Packet v2 validates successfully

update the same Ideas row with:

- `final_title`
- `draft_url`
- `ig_packet_json`
- `status = DRAFTED`

Do not require a new `ncs_score`.

Do not overwrite an existing legacy `ncs_score` merely because the new workflow does not use scoring.

Never populate `published_url` or set `PUBLISHED`.

## 14. Meaning of DRAFTED

`DRAFTED` now means:

- the production is fully developed internally
- a durable production treatment exists
- a schema-valid social release packet exists
- the complete package is ready for asset generation and human review

It does not mean:
- a long-form article exists
- an editorial score exists
- a real cast exists
- the production is approved for publication

## 15. Optional later long-form path

If a human later wants a full article, website feature, or email edition:

1. load frozen canon
2. run one purpose-built long-form writer call
3. optionally run one review call only if the article is important enough to justify it
4. persist separately
5. regenerate the social packet only if canon, title, or public campaign direction materially changes

Do not place long-form drafting back into the required Generation path.

## 16. Legacy prompts

The following files may remain in the repository for historical use, calibration, or explicit later workflows, but are not part of normal v3 Generation:

- former Production Developer
- Story Challenger
- former Canon Builder
- Casting Director
- Edition Architect
- Edition Writer
- Forensic Editor
- Revision Writer

Do not delete them until the new workflow has been tested and the human chooses to retire legacy paths.

## 17. Execution summary

Return:
- source commit SHA
- idea ID
- final title
- resolved format
- Production Builder success
- optional research status
- treatment URL
- IG Packet QA result
- final status
- unresolved hard warnings

The workflow should normally require only two creative model calls.
