# Never Coming Soon

Never Coming Soon is an entertainment and media brand for the best movies and tv shows that do not exist.

This repository contains the durable creative governance, prompt contracts, structured schemas, workflow specifications, and approved calibration material used by the Never Coming Soon system.

## Core product

The production is the product.

A carousel, poster, short video, long-form article, website feature, or email can all be different ways of experiencing the same fictional movie or show.

The current consumer-facing priority is social-first packaging.

The target reaction is:

**Oh shit. I would actually watch this.**

## System architecture

Never Coming Soon uses distinct creative workflows.

1. **Ideation** finds, remembers, compares, curates, expands, and Development-Selects fertile concepts.
2. **Generation** turns one selected concept into a complete internal production canon, persists a readable production treatment, creates the canonical six-slide social release packet, and delivers the package for asset generation and human judgment.
3. **Optional research** is run only when explicitly requested or materially required.
4. **Optional long-form editorial** is created later only when a human actually wants an article, website feature, or email edition.
5. **Visual production** executes the generated social asset packet.
6. **Publishing and growth** remain downstream human-controlled systems.

The normal Generation path is intentionally lean.

## Current Generation path

Normal paid-model path:

1. Production Builder
2. Social Release Builder

Optional:
- Research Grounder
- one Production Builder rerun after material research
- later long-form writer
- later review or revision only when justified

The former Production Developer, Story Challenger, Canon Builder, Casting Director, Edition Architect, Edition Writer, Forensic Editor, and Revision Writer files may remain in the repository for legacy or explicit later workflows, but they are not part of the normal v3 path.

## Source of truth

- `Governance/` contains durable creative, visual, social, editorial, and data standards.
- `Prompts/Ideation/` contains Ideation prompts.
- `Prompts/Generation/01-production-builder.*` contains the streamlined Production Builder.
- `Prompts/Generation/10-ig-asset-packet-builder.*` contains the Social Release Builder.
- `Schemas/` contains machine-readable contracts.
- `WorkflowSpecs/ideation-workflow-v1.md` contains the Ideation specification.
- `WorkflowSpecs/generation-workflow-v1.md` contains the current Generation specification despite the legacy filename.
- `Examples/Gold/` contains approved long-form writing calibration for optional editorial work.

GitHub is the durable source of truth.

n8n orchestrates the workflows.

Google Sheets stores catalog and lifecycle state.

Google Drive stores the durable production treatment and optional later editorial artifacts.

## Ideas state

The system uses one canonical `Ideas` tab and one row per concept.

Canonical downstream lifecycle:

`DEVELOPMENT_SELECT -> DRAFTED -> PUBLISHED`

There is no intermediate `GENERATING` Sheet status.

Generation uses `idea_id` as its durable identifier.

## Meaning of DRAFTED

A successful v3 Generation run updates the same Ideas row with:

- `final_title`
- `draft_url`
- `ig_packet_json`
- `status = DRAFTED`

`draft_url` is a legacy field name retained for compatibility and points to the current production treatment.

`ncs_score` is not required by the normal social-first path. Existing historical scores should not be erased.

`DRAFTED` means:
- complete internal canon was created
- a durable production treatment exists
- a schema-valid social release packet exists
- the production is ready for asset generation and human review

It does not mean published.

## Social release packet

`ig_packet_json` remains the canonical downstream media handoff.

Version:

`ncs_ig_v2`

Its locked six-slide spine is:

1. Hook
2. Premise
3. Characters
4. The Movie
5. Poster
6. NCS Close

### Slide 1

Slide 1 is intentionally simple and text-first.

It clearly says:
- MOVIE IDEA or SHOW IDEA
- final title
- one-sentence hook
- small Never Coming Soon signature

It is not a poster.

### Slide 2

Premise.

A cinematic glimpse plus clear social copy that gives the setup generously.

### Slide 3

Featured Characters.

Two characters by default, three for genuinely ensemble-driven concepts, four only when necessary.

The default visual treatment is an NCS editorial character study or casting-room sketch, not photoreal fake actors.

### Slide 4

The Movie.

An immersive, image-dominant slide that sells what watching the production feels like without summarizing the plot.

### Slide 5

Poster payoff.

This is where full campaign-style key art belongs.

### Slide 6

NCS close.

Participation question first, then brand identity and the locked slogan:

**the best movies and tv shows that don't exist.**

A link or long-form action is secondary and only appears when a real destination exists.

## Image-model boundary

The image model is an executor, not an editor.

The packet decides:
- exact copy
- slide order
- character selection
- character count
- story emphasis
- reveal boundaries
- poster tagline
- CTA
- visual continuity

The image model receives execution-ready visual prompts and exercises creativity inside those constraints.

Exact text and the approved logo should be composited deterministically whenever practical.

## Long-form editorial

Long-form writing remains available because some productions may benefit from a deeper website or email experience.

It is no longer required merely to produce the social packet.

If a human wants a full article later, use the frozen canon or production treatment as source and pay for only the writing and review steps that are actually valuable.

## Public integrity

Published work must never expose internal NCS process language.

Use:

- `Governance/ncs-publication-integrity-standard.md`

Backstage tooling is not the product.

## Core quality posture

Never Coming Soon is trying to create this reaction:

**Oh shit. I would actually watch this.**

The standard is desire, specificity, human pull, genuine genre pleasure, and the feeling that the production somehow already exists.
