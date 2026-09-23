# Never Coming Soon

Never Coming Soon is an entertainment and media brand for the best movies and tv shows that do not exist.

The production is the product.

A carousel, poster, short video, article, website feature, or email can all be different ways of experiencing the same fictional movie or show.

The current consumer-facing priority is social-first packaging.

The target reaction is:

**Oh shit. I would actually watch this.**

## Current operating model

Never Coming Soon now uses one unified n8n creative workflow.

The workflow runs:

`Ideation -> duplicate control -> curation -> expansion -> Development Select -> Production Builder -> Production Treatment -> Social Release Builder -> DRAFTED`

There is no separate required Generation workflow.

Asset generation is manual and intentionally outside n8n.

Long-form editorial is optional later work.

## Source of truth

- `Governance/` contains durable creative, visual, social, editorial, and data standards.
- `Prompts/Ideation/` contains Ideation-stage prompts.
- `Prompts/Generation/01-production-builder.*` contains the streamlined Production Builder. The directory name is retained for repository organization.
- `Prompts/Generation/10-ig-asset-packet-builder.*` contains the Social Release Builder.
- `Schemas/` contains machine-readable structured-output contracts.
- `WorkflowSpecs/ideation-workflow-v1.md` is the canonical unified n8n workflow spec despite the legacy filename.
- `WorkflowSpecs/generation-workflow-v1.md` is a compatibility notice and must not be implemented as a second normal workflow.
- `Examples/Gold/` contains approved long-form writing calibration for optional later editorial work.

GitHub is the durable source of truth.

n8n orchestrates the unified creative workflow.

Google Sheets stores catalog and lifecycle state.

Google Drive stores durable Production Treatments and optional later editorial artifacts.

## Normal creative path

Ideation retains its current quality-control system:

1. Ideation Director
2. Seed Generator
3. Duplicate Auditor
4. Concept Curator
5. Idea Expander
6. second Duplicate Auditor
7. Development Selector

For each concept that becomes `DEVELOPMENT_SELECT`, the same n8n execution continues with only two additional creative calls:

8. Production Builder
9. Social Release Builder

Optional research may run only when explicitly requested or materially necessary.

The former Production Developer, Story Challenger, Canon Builder, Casting Director, Edition Architect, Edition Writer, Forensic Editor, and Revision Writer files may remain for legacy or optional later use, but they are not part of the normal social-first path.

## Production Builder

The Production Builder turns the Development Packet into complete internal canon.

It owns:

- final title
- final format
- genre
- logline
- world
- characters
- relationships
- complete story
- actual ending
- signature scenes
- genre delivery
- public unresolved value
- series engine and season material when relevant
- continuity facts

No article is required.

No real-actor casting step is required.

No editorial score is required.

## Production Treatment

The workflow renders a human-readable Production Treatment deterministically from the final Canon Bible.

This does not require another model call.

The treatment is persisted to Google Drive.

The legacy `draft_url` Sheet column points to this treatment.

## Social release packet

`ig_packet_json` is the canonical public-release handoff.

Version:

`ncs_ig_v2`

Its locked six-slide spine is:

1. Hook
2. Premise
3. Characters
4. The Movie
5. Poster
6. NCS Close

Every slide contains:

- `Never Coming Soon` footer
- exact page number in `0X / 06` format

### Slide 1: Hook

Brutally simple and text-first.

It clearly communicates:

- MOVIE IDEA or SHOW IDEA
- final title
- one-sentence hook

It is not a poster.

### Slide 2: Premise

A cinematic glimpse plus exact social copy that gives the setup generously.

The established premise-copy calibration remains roughly 65 to 110 words with natural paragraphing.

### Slide 3: Characters

Two characters by default, three for genuinely ensemble-driven concepts, four only when necessary.

Default visual system:

- one separate illustrated editorial portrait per featured character
- graphite / ink / loose watercolor language
- no real actor likenesses
- no rounded cards or boxes
- open editorial layout
- alternating portrait/copy placement
- no decorative object in the middle
- `THE STARS` as the preferred default section title

Each character receives its own exact `portrait_prompt`.

### Slide 4: The Movie

The most immersive slide.

It sells what watching the production feels like without becoming a plot summary.

### Slide 5: Poster

The poster payoff.

The fictional production owns the central poster visual and title treatment.

### Slide 6: NCS Close

Participation-first brand close.

Locked slogan:

**the best movies and tv shows that don't exist.**

A link or long-form action is secondary and only appears when a real destination exists.

## Manual asset generation

n8n stops after persisting the validated `ig_packet_json`.

Asset generation happens manually in a separate chat.

The packet is designed to be pasted there verbatim.

The asset-generation model should execute the packet rather than decide:

- slide order
- copy
- featured characters
- character count
- story emphasis
- reveal strategy
- poster tagline
- CTA

Character continuity is only a safeguard when the same person genuinely appears in more than one asset.

Do not repeat faces merely for consistency.

## Ideas lifecycle

The system uses one canonical `Ideas` tab and one row per concept.

Lifecycle:

`RAW -> DEVELOP / PROMISING / HOLD / DUPLICATE -> DEVELOPMENT_SELECT -> DRAFTED -> PUBLISHED`

There is no durable `GENERATING` state.

A selected concept remains `DEVELOPMENT_SELECT` until its full production package succeeds.

A successful package writes:

- `final_title`
- `draft_url`
- `ig_packet_json`
- `status = DRAFTED`

`ncs_score` is a legacy optional field and is not required by the normal social-first path.

`PUBLISHED` remains human-controlled.

## Meaning of DRAFTED

`DRAFTED` means:

- complete internal canon exists
- durable Production Treatment exists
- schema-valid social release packet exists
- production is ready for manual asset generation and human judgment

It does not mean:

- an article exists
- a score exists
- actors have been cast
- carousel assets have been rendered
- the production has been approved for publication

## Long-form editorial

Long-form writing remains available for productions that deserve a deeper website, email, or editorial experience.

It is not generated automatically.

If desired later, use the frozen Production Treatment or Canon Bible as source and pay only for the writing and review work that is actually valuable.

## Public integrity

Published work must never expose internal NCS process language.

Use:

`Governance/ncs-publication-integrity-standard.md`

Backstage tooling is not the product.

## Core quality posture

Never Coming Soon is trying to create this reaction:

**Oh shit. I would actually watch this.**

The standard is desire, specificity, human pull, genuine genre pleasure, and the feeling that the production somehow already exists.
