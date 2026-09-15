# Never Coming Soon

Never Coming Soon is an imaginary entertainment studio and media publication for the best movies and shows that do not exist.

This repository contains the durable creative governance, prompt contracts, structured schemas, workflow specifications, and approved calibration examples used by the Never Coming Soon system.

## System architecture

Never Coming Soon uses distinct creative workflows.

1. **Ideation** finds, remembers, compares, curates, lightly expands, and Development-Selects fertile concepts.
2. **Generation** turns a selected concept into a real internal movie or show, optionally grounds it with research, internally challenges and freezes canon, casts it, architects the public edition, drafts it, runs deterministic diagnostics, cold-reviews it once, assigns a holistic NCS score, builds the canonical IG/social handoff, and delivers the complete package for human judgment and downstream media asset generation.
3. **Later revision / redevelopment** is explicit and human-selected. It may run targeted prose revision, edition restructuring, canon redevelopment, rescoring when needed, and IG packet regeneration when the final public object materially changes.
4. **Visual production** executes the generated social asset packet.
5. **Publishing and growth** remain downstream human-controlled systems. Generation never auto-publishes.

The workflows are intentionally separate.

Ideation explains why an idea deserves development. Generation owns the actual storytelling and may materially improve title, format, characters, relationships, setting, plot, scenes, ending, casting, and other provisional choices.

The handoff object from Ideation to Generation is the **Development Packet**.

The core Generation principle is:

**Develop the production before writing the article.**

The cost/quality principle is:

**Spend premium model calls where they create creative quality or a required downstream artifact. Do not automatically revise every draft before a human has read it.**

## Current Generation creative path

Normal paid-model path:

1. Production Developer
2. Research Grounder only when genuinely needed
3. Canon Builder with internal independent challenge
4. Casting Director
5. Edition Architect
6. Edition Writer
7. Forensic Editor
8. IG Asset Packet Builder

The standalone Story Challenger is not part of the normal Generation path. Its critical function is folded into Canon Builder before Canon Freeze.

Automatic Revision Writer, EDITION/CANON rescue loops, and duplicate final Forensic review are also not part of normal Generation.

The IG Asset Packet Builder remains in normal Generation because `ig_packet_json` is required by downstream media asset generation.

## Source of truth

- `Governance/` contains durable creative, editorial, visual, social, and data standards.
- `Prompts/Ideation/` contains Ideation agent prompt pairs.
- `Prompts/Generation/` contains generation-stage prompts plus retained prompts for later/optional stages.
- `Schemas/` contains machine-readable structured-output contracts.
- `WorkflowSpecs/ideation-workflow-v1.md` contains the n8n Ideation specification.
- `WorkflowSpecs/generation-workflow-v1.md` contains the current n8n Generation specification despite the legacy filename.
- `Examples/Gold/` contains explicitly approved writing-calibration examples.

GitHub is the source of truth for durable system behavior.

n8n orchestrates the workflows.

Google Sheets stores the catalog and lifecycle state.

Google Drive stores human-readable draft artifacts.

## Ideas state

The system uses one canonical `Ideas` tab and one row per concept across the lifecycle.

Canonical downstream lifecycle:

`DEVELOPMENT_SELECT` -> `DRAFTED` -> `PUBLISHED`

There is no intermediate `GENERATING` Sheet status in the current architecture.

For a normal run, the row remains `DEVELOPMENT_SELECT` until the complete generated package succeeds. For forced redevelopment, a prior `DRAFTED` row remains `DRAFTED` with its successful final fields intact until the replacement package succeeds.

`PUBLISHED` is human-controlled.

Generation uses `idea_id` as its sole durable identifier. There is no required `production_id` and no separate Productions state layer.

Intermediate Generation artifacts remain in n8n execution memory.

If duplicate-run protection is needed, solve it at the n8n execution/orchestration level rather than with another durable lifecycle status.

## Meaning of DRAFTED

A successful Generation run updates the same Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` means a complete scored article and schema-valid media/social handoff exist and were persisted successfully.

It does **not** require:

- `ncs_score >= 8.0`
- a Forensic `revision_route` of `NONE`
- every editorial note to be resolved
- automatic publication approval

The score records quality. The status records artifact lifecycle.

## Human review and editorial sufficiency

The automated Generation system should create a strong first object, diagnose it honestly, create the required media handoff, and stop.

A 7.x draft can be a valid generated object when the concept is good, the article is coherent, and remaining weaknesses are matters for later human selection or polish.

The Forensic Editor's route is advisory during normal Generation.

A human may later choose:

- use as-is
- targeted prose revision
- edition restructuring
- canon redevelopment
- no publication

This is intentionally cheaper and less likely to polish personality out of good work than automatically revising every generated draft.

## Relationship-driven productions

Romance, romantic comedy, second-chance love, and other central two-person relationship stories use:

- `Governance/ncs-relationship-story-standard.md`

The standard requires actual chemistry in behavior, credible breakup logic for second-chance romance, bilateral life stakes when relevant, and dignified treatment of new partners.

A premise mechanism may create proximity. It cannot substitute for the relationship itself.

## Scene ownership

Edition Architecture assigns substantial public sequences one primary section through `scene_ownership_plan`.

For film, THE MOVIE and THE SCENES should not fully stage the same event.

For television, THE SEASON and THE EPISODES should operate at different zoom levels.

THE SEASON tracks concrete macro change. THE EPISODES gives selected specific memorable stories.

Deterministic diagnostics can flag likely overlap, while the Forensic Editor makes the editorial judgment.

## Casting memory

Casting history is awareness, not a blacklist.

At minimum, approved Gold-example lead casts should be visible to the Casting Director so the system does not immediately reuse the same lead performer out of habit.

Casting may use a cheaper capable creative model than the major development/writing/review stages when practical.

Public THE CAST copy only includes roles with an actual selected performer.

## Social asset handoff

`ig_packet_json` is the canonical handoff to the image and social asset workflow and remains part of successful Generation.

Its governance lives in:

- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`

Its schema lives in:

- `Schemas/ig-asset-packet.schema.json`

The locked three-slide spine remains:

1. Cover / Poster
2. Premise
3. NCS Close

The exact final packet written to Sheets must pass schema validation and Social QA after all normalization or repair.

If later human-selected revision materially changes the final title, public article, canon, or campaign direction, regenerate the IG packet so downstream media assets stay synchronized with the actual production.

## Public integrity

Published work must never expose internal NCS process language.

Use:

- `Governance/ncs-publication-integrity-standard.md`

Internal editorial concepts may guide the system backstage but must not appear as public annotations or spoiler-management commentary.

## Television

Television has dedicated format governance:

- `Governance/ncs-television-editorial-standard.md`

It is designed to prevent a series article from reading like a show bible, rules document, or duplicated season recap plus episode guide.

Television still needs a formally locked Gold example before TV craft should be considered fully calibrated.

## Writing calibration

The house voice is governed by:

- `Governance/ncs-voice-constitution.md`

Approved film Gold examples:

- `Examples/Gold/film-01-the-tell.md`
- `Examples/Gold/film-02-clearance.md`

Gold examples teach voice, craft, section behavior, and editorial judgment. They are never story templates or minimum-score requirements.

## Core quality posture

Never Coming Soon is trying to create this reaction:

**Oh shit. I would actually watch this.**

The standard is not maximum strangeness, maximum plot density, maximum cleverness, or maximum automated revision.

The standard is desire, specificity, human pull, genuine genre pleasure, and the feeling that the production somehow already exists.
