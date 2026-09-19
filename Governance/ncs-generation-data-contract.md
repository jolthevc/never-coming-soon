# Never Coming Soon
## Generation Data Contract v3.0

## 1. Purpose

This document defines the durable data contract for the streamlined social-first Generation workflow.

The normal workflow develops one complete internal production, persists a readable treatment, creates the canonical social release packet, and stops.

A long-form article and editorial score are optional later artifacts, not required Generation outputs.

## 2. Required source

Generation begins from one Ideas row with:

- `idea_id`
- `status = DEVELOPMENT_SELECT`
- nonblank `development_packet_json`

Optional:
- `human_notes`
- explicit research questions
- explicit human campaign notes

## 3. Required generated objects

Normal Generation produces:

1. `canon_bible`
2. deterministic production treatment in Google Drive
3. `ig_packet_json`

Only `ig_packet_json` is persisted directly in the Sheet as structured JSON.

The Canon Bible may remain in execution memory because the durable treatment preserves its content for human use. If orchestration later adds a canonical production-store layer, the canon may also be persisted there without changing the Ideas row.

## 4. Canon contract

The Production Builder returns:

`Schemas/canon-bible.schema.json`

The canon owns:
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
- series engine
- season-one material
- continuity facts

The canon is internal and complete.

The social release packet is selective and public-facing.

## 5. Research

Research is optional.

If explicit research is run, its output must be factual support rather than prose.

If research materially changes canon, rerun Production Builder once with the research packet.

Do not create recursive research loops.

## 6. Production treatment

The production treatment is rendered deterministically from the final Canon Bible.

It is not a generated article and should not require a model call.

The legacy Sheet column `draft_url` remains in use for compatibility and points to this treatment.

The treatment should be readable enough for human review and future optional long-form writing.

## 7. Social handoff contract

A successful Generation run produces `ig_packet_json` matching:

`Schemas/ig-asset-packet.schema.json`

and governed by:

- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`

The packet is required for downstream asset generation.

Version:

`ncs_ig_v2`

Required architecture:

1. Hook
2. Premise
3. Characters
4. The Movie
5. Poster
6. NCS Close

The packet also contains:
- campaign brief
- visual continuity
- exact launch caption
- exact public copy
- exact featured-character selection
- execution-ready visual prompts

The image workflow must treat this packet as the editorial source of truth.

## 8. Packet validation

Validate the exact final object that will be stringified into the Ideas cell.

Hard requirements include:
- valid schema
- exact v2 version
- final title and format match canon
- six slides exactly
- correct slide types
- correct MOVIE IDEA / SHOW IDEA mapping
- 2 to 4 featured characters
- nonblank visual continuity
- nonblank required image prompts
- nonblank caption
- no em dash character in public copy
- no backstage technology language
- no false participation claims

Allow one packet-only repair.

If the final packet remains invalid, do not set `DRAFTED`.

## 9. Final Ideas update

After successful treatment persistence and packet validation, update:

- `final_title`
- `draft_url`
- `ig_packet_json`
- `status = DRAFTED`

`ncs_score` is not required by the v3 normal path.

Do not overwrite a historical score with a blank or synthetic value.

Generation does not write `published_url`.

Generation does not set `PUBLISHED`.

## 10. DRAFTED meaning

`DRAFTED` means:

- complete internal production canon was successfully created
- a durable human-readable treatment exists
- a schema-valid social release packet exists
- the package is ready for asset generation and human judgment

It does not require:
- a public article
- a holistic editorial score
- real actor casting
- automated review
- publication approval

## 11. Optional later article

A long-form article, website feature, or email edition is generated only through explicit later human action.

That optional path may use the frozen production treatment or Canon Bible as source.

Do not require the old Architect -> Writer -> Forensic chain merely to create a social release.

## 12. Force redevelopment

For `force_redevelopment = true`:

- keep the prior successful final fields intact during the run
- create replacement canon, treatment, and packet
- replace final fields only after the entire replacement package succeeds

Do not destroy the last good package before the new one is valid.

## 13. Human authority

The automated system develops and packages the fictional production.

Humans still decide:
- whether to release it
- whether to create long-form editorial
- whether to revise canon
- whether to regenerate social assets
- whether to publish

The normal path should be lean enough that strong ideas can be developed and packaged repeatedly without paying for unused editorial layers.
