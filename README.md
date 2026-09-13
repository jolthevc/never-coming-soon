# Never Coming Soon

Never Coming Soon is an imaginary entertainment studio and media publication for the best movies and shows that do not exist.

This repository contains the durable creative governance, prompt contracts, structured schemas, workflow specifications, and approved calibration examples used by the Never Coming Soon system.

## System architecture

Never Coming Soon uses distinct creative workflows.

1. **Ideation** finds, remembers, compares, curates, lightly expands, and Development-Selects fertile concepts.
2. **Generation** turns a selected concept into a real internal movie or show, optionally grounds it with research, challenges it, freezes canon, casts it, architects the public edition, drafts it, forensically reviews it, revises material problems when useful, assigns a final holistic NCS score, creates the social asset handoff, and delivers the final article to Google Drive.
3. **Visual production** executes the prepared social asset packet after text canon and the final edition are stable.
4. **Publishing and growth** remain downstream human-controlled systems. Generation never auto-publishes.

The workflows are intentionally separate.

Ideation explains why an idea deserves development. Generation owns the actual storytelling and may materially improve title, format, characters, relationships, setting, plot, scenes, ending, casting, and other provisional choices.

The handoff object from Ideation to Generation is the **Development Packet**.

The core Generation principle is:

**Develop the production before writing the article.**

The core review principle is:

**Fix material problems, then stop.**

Generation is not designed to auto-revise every article into theoretical perfection.

## Source of truth

- `Governance/` contains durable creative, editorial, visual, social, and data standards.
- `Prompts/Ideation/` contains Ideation agent prompt pairs.
- `Prompts/Generation/` contains Generation and social-handoff prompt pairs.
- `Schemas/` contains machine-readable structured-output contracts.
- `WorkflowSpecs/ideation-workflow-v1.md` contains the n8n Ideation specification.
- `WorkflowSpecs/generation-workflow-v1.md` contains the current n8n Generation specification despite the legacy filename.
- `Examples/Gold/` contains explicitly approved writing-calibration examples.

GitHub is the source of truth for durable system behavior.

n8n orchestrates the workflows.

Google Sheets stores the catalog and lifecycle state.

Google Drive stores final human-readable draft artifacts.

## Ideas state

The system uses one canonical `Ideas` tab and one row per concept across the lifecycle.

Canonical downstream lifecycle:

`DEVELOPMENT_SELECT` -> `GENERATING` -> `DRAFTED` -> `PUBLISHED`

`PUBLISHED` is human-controlled.

Generation uses `idea_id` as its sole durable identifier. There is no required `production_id` and no separate Productions state layer in the current architecture.

Intermediate Generation artifacts remain in n8n execution memory.

## Meaning of DRAFTED

A successful Generation run updates the same Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` is an artifact-existence state.

It means the generated package was completed and persisted successfully.

It does not require `ncs_score >= 8.0`.

It does not mean every editorial note was resolved.

It does not mean the article should automatically publish.

The score records quality. The status records lifecycle.

Keeping those jobs separate makes the dashboard easier to understand and lets the human editor compare strong, middling, and weak completed drafts without pretending failed state.

## Editorial sufficiency

The automated review system should improve material defects and preserve strong ideas.

A 7.x draft can be a perfectly valid generated object when the concept is good, the article is coherent, and remaining weaknesses are matters of taste or polish.

The workflow should not reopen canon or rewrite large portions of a good draft merely to push a score over an arbitrary threshold.

Deep automated rescue remains intentionally limited.

## Relationship-driven productions

Romance, romantic comedy, second-chance love, and other central two-person relationship stories use:

- `Governance/ncs-relationship-story-standard.md`

The standard requires actual chemistry in behavior, credible breakup logic for second-chance romance, bilateral life stakes when relevant, and dignified treatment of new partners.

A premise mechanism may create proximity. It cannot substitute for the relationship itself.

## Scene ownership

Edition Architecture assigns substantial public sequences one primary section through `scene_ownership_plan`.

For film, THE MOVIE and THE SCENES should not fully stage the same event.

For television, THE SEASON and THE EPISODES should operate at different zoom levels.

THE SEASON tracks macro change. THE EPISODES gives specific memorable stories.

A longer television season usually does not need every episode represented publicly.

Deterministic diagnostics can flag likely overlap, while the Forensic Editor makes the editorial judgment.

## Casting memory

Casting history is awareness, not a blacklist.

At minimum, approved Gold-example lead casts should be visible to the Casting Director so the system does not immediately reuse the same lead performer out of habit.

Public THE CAST copy only includes roles with an actual selected performer.

## Social asset handoff

`ig_packet_json` is the canonical handoff to the image and social asset workflow.

Its governance lives in:

- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`

Its schema lives in:

- `Schemas/ig-asset-packet.schema.json`

The locked three-slide spine is:

1. Cover / Poster
2. Premise
3. NCS Close

The packet also contains one short social caption.

The structured format field must use exactly `FILM`, `SERIES`, or `LIMITED_SERIES`.

The exact final object written to Sheets must pass schema validation after all normalization or repair.

Campaign coherence should come from art direction, not from repeating the same literal hero object on all three slides.

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
