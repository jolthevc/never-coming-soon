# Never Coming Soon
## Production Packaging Data Contract v4.0

> Filename retained for loader compatibility.

## 1. Purpose

This document defines the production-development and social-packaging contract used inside the single unified Never Coming Soon n8n workflow.

There is no separate required Generation workflow.

The unified workflow reaches this contract after a concept becomes `DEVELOPMENT_SELECT`.

The production-packaging phase:

1. builds complete internal canon
2. renders a readable Production Treatment
3. creates the canonical three-slide social release packet
4. advances the same Ideas row to `DRAFTED`

A long-form article and editorial score are optional later artifacts.

## 2. Required source

The production-packaging phase begins with one selected Ideas row containing:

- `idea_id`
- `status = DEVELOPMENT_SELECT`
- nonblank `development_packet_json`

Optional:

- `human_notes`
- explicit research questions
- `human_campaign_notes`

## 3. Required generated objects

The normal phase produces:

1. `canon_bible_json`
2. deterministic Production Treatment
3. `ig_packet_json`

Only `ig_packet_json` is persisted directly to the Sheet as structured JSON.

The Canon Bible remains in execution memory for the successful pass. Its substantive content is preserved in the durable Production Treatment.

## 4. Production Builder contract

Use:

- `Prompts/Generation/01-production-builder.system.md`
- `Prompts/Generation/01-production-builder.user.md`
- `Schemas/canon-bible.schema.json`

The Canon Bible owns:

- final title
- final format
- genre
- logline
- core promise
- creative kernel
- world
- characters
- relationships
- complete story
- actual ending
- signature scenes
- genre delivery
- public unresolved value
- public genre demonstrations
- series engine and season material when relevant
- continuity facts

The canon is internal and complete.

The social release packet is selective and public-facing.

## 5. Research

Research is optional.

Run it only when:

- explicitly requested, or
- a configured high-confidence rule identifies factual uncertainty that materially affects canon

If research materially changes the production:

1. produce the research packet
2. rerun Production Builder once with that packet
3. freeze the second canon

Do not create recursive research loops.

## 6. Production Treatment

Render the Production Treatment deterministically from final Canon Bible.

Do not spend another creative model call.

The treatment should include:

- final title
- format and genre
- logline
- core promise
- creative kernel
- world
- characters
- central relationships
- complete internal story
- signature scenes
- genre delivery
- series engine / season material when relevant
- continuity facts

Keep the rendered treatment in execution memory until `ig_packet_json` also validates.

Then persist to:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE] - Production Treatment`

The legacy Sheet field `draft_url` points to this document.

## 7. Social Release Builder contract

Use:

- `Prompts/Generation/10-ig-asset-packet-builder.system.md`
- `Prompts/Generation/10-ig-asset-packet-builder.user.md`
- `Schemas/ig-asset-packet.schema.json`

Governance:

- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`
- `Governance/ncs-publication-integrity-standard.md`
- `Governance/ncs-voice-constitution.md`

Input:

- final Canon Bible
- optional human campaign notes

Do not require:

- article prose
- casting plan
- editorial review
- `ncs_score`

Output:

`ig_packet_json`

Version:

`ncs_ig_v3`

## 8. Social packet contract

Required three-slide architecture:

1. Hook
2. Plot
3. Poster

The packet contains:

- exact launch caption
- exact Slide 1 hook copy
- exact Slide 2 plot copy with paragraph-ready spacing
- exact Slide 3 poster copy
- execution-ready poster art direction and image prompt
- fixed `Never Coming Soon` footer on every slide
- exact page number on every slide

The packet is intended to be copied verbatim into a separate manual asset-generation chat.

There is no n8n image-generation stage.

## 9. Packet validation

Validate the exact final object that will be stringified into the Ideas cell.

Hard requirements include:

- valid schema
- `version = ncs_ig_v3`
- final title and format match Canon Bible
- three slides exactly
- Slide 1 type = `hook`
- Slide 2 type = `plot`
- Slide 3 type = `poster`
- FILM -> MOVIE IDEA
- SERIES / LIMITED_SERIES -> SHOW IDEA
- every slide footer = `Never Coming Soon`
- page numbers exactly `01 / 03`, `02 / 03`, and `03 / 03`
- Slide 1 hook copy nonblank
- Slide 2 label = `THE PLOT`
- Slide 2 body copy nonblank and paragraph-ready
- Slide 3 poster fields and image prompt nonblank
- nonblank caption
- no em dash character in public copy
- no backstage technology language
- no false participation claims

Allow one packet-only repair.

If the final packet remains invalid, do not set `DRAFTED`.

Do not rerun Production Builder merely because packet formatting failed.

## 10. Final Ideas update

After:

- Canon Bible validates
- IG Packet validates
- Production Treatment persists successfully

update:

- `final_title`
- `draft_url`
- `ig_packet_json`
- `status = DRAFTED`

`ncs_score` is not required.

Do not overwrite a historical score with blank or synthetic data.

Do not write `published_url`.

Do not set `PUBLISHED`.

## 11. DRAFTED meaning

`DRAFTED` means:

- complete internal production canon was built
- durable Production Treatment exists
- schema-valid social release packet exists
- package is ready for manual asset generation and human judgment

It does not require:

- public article
- editorial score
- real actor casting
- automated review
- rendered carousel assets
- publication approval

## 12. Failure behavior

If any hard production-package stage fails:

- keep the row in `DEVELOPMENT_SELECT`
- preserve `development_packet_json`
- do not write partial final fields
- allow sibling selected concepts in the same unified workflow execution to continue when safe

## 13. Optional redevelopment

For explicit redevelopment of a DRAFTED concept:

- preserve the prior successful final fields and Drive artifact during the run
- create replacement canon, treatment, and packet
- replace final fields only after the replacement package fully succeeds

Do not destroy the last good package first.

## 14. Optional later long-form editorial

A long-form article, website feature, or email edition is created only through explicit later human action.

That later work may use the Production Treatment or frozen Canon Bible as source.

Do not require the former Architect -> Writer -> Forensic chain merely to produce the social release.

## 15. Human authority

The automated unified workflow develops and packages the fictional production.

Humans still decide:

- whether to release it
- how to execute the assets manually
- whether to revise canon
- whether to revise the packet
- whether to create long-form editorial
- whether to publish

The production-packaging phase should remain lean enough to run routinely without paying for unused editorial layers.
