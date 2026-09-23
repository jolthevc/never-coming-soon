# Never Coming Soon
## Internal Dashboard Control Plane v2.0

## Purpose

Build a lightweight internal control surface for steering the single Never Coming Soon creative workflow.

The dashboard is a thin client.

n8n owns orchestration.
The Ideas Google Sheet owns durable catalog and lifecycle state.
Google Drive owns durable production treatments.
GitHub owns governance, prompts, schemas, and workflow specifications.

There is no separate Productions state layer and no separate normal Generation workflow.

## Primary dashboard actions

### Explore

One large creative-direction input.

Optional controls:

- format: ANY, FILM, SERIES, LIMITED_SERIES
- genre: ANY or free text
- ideation mode: AUTO or one canonical Ideation mode
- anchor strength: LOOSE, CENTERED, STRICT

Submitting Explore invokes DIRECTED mode of the unified workflow.

### Run General Ideation

Invokes GENERAL mode of the unified workflow using the normal target seed count, default 30.

### Quick Sparks

When the dashboard loads, show five disposable one-sentence concepts.

Quick Sparks are not written to Sheets, assigned idea IDs, or included in catalog memory.

Each Spark shows:

- one-sentence concept
- provisional format
- provisional genre
- Develop this
- Dismiss

Develop this invokes CONCEPT_INTAKE for that exact concept.

### Ideas Queue

Read the Ideas tab and display at minimum:

- idea_id
- working_title
- final_title when available
- format
- genre
- status
- premise
- human_notes
- draft_url when available
- published_url when available
- whether ig_packet_json exists

Useful actions:

- edit human notes
- inspect the Development Packet
- human Development Select override when appropriate
- explicitly redevelop an existing selected or drafted idea
- open Production Treatment when draft_url exists
- inspect or copy ig_packet_json

Do not build asset generation controls into the dashboard.

## Unified lifecycle

The same Ideas row moves through:

`RAW -> DEVELOP / PROMISING / HOLD / DUPLICATE -> DEVELOPMENT_SELECT -> DRAFTED -> PUBLISHED`

There is no durable `GENERATING` status.

Newly Development-Selected concepts normally continue directly through Production Builder and Social Release Builder in the same n8n execution.

A selected production remains `DEVELOPMENT_SELECT` until the complete production package succeeds.

`PUBLISHED` remains human-controlled.

## DRAFTED view

If a dedicated view is useful, derive it from Ideas rows where status is `DRAFTED` or `PUBLISHED`.

Display:

- idea_id
- final_title
- format
- genre
- status
- draft_url
- published_url
- whether ig_packet_json exists

The legacy `ncs_score` may be shown when present, but it is not required by the current workflow.

## Unified workflow entry contract

Supported run types:

- GENERAL
- DIRECTED
- CONCEPT_INTAKE

Common request fields:

- `run_type`
- `target_seed_count`
- `human_direction`
- `concept_text`
- `target_format`
- `target_genre`
- `preferred_ideation_mode`
- `anchor_strength`
- `human_notes`
- `human_campaign_notes`

The detailed contract lives in:

`WorkflowSpecs/ideation-entry-modes-v1.md`

## Redevelopment

An advanced action may invoke the same unified workflow with:

- explicit `idea_id`
- `force_redevelopment = true`

Require confirmation.

The prior successful package must remain intact until the replacement package fully succeeds.

Do not implement redevelopment as a second n8n workflow.

## Asset generation

Asset generation is intentionally manual.

The dashboard may provide a convenient copy/view action for `ig_packet_json`, but it should not render slides, call image models, generate posters, or export carousels.

## Security and architecture

Recommended application stack: Next.js, TypeScript, App Router, and a small server-side API layer.

The browser must not hold Google, OpenAI, Anthropic, GitHub, or n8n credentials.

Use:

Dashboard browser -> dashboard server routes -> authenticated n8n endpoint -> unified creative workflow and data systems

Do not reimplement creative logic in the web app.

## Visual direction

The dashboard should feel like a clean internal studio console rather than SaaS analytics software.

House palette:

- Midnight Navy: #08192F
- Warm Ivory: #F5EEDF
- Reel Orange: #F24B2C
- Electric Cobalt: #315CFF
- Slate: #202632

Use Midnight Navy and Warm Ivory as dominant surfaces.

Avoid film-industry cliches such as clapperboards, film strips, projectors, red velvet, gold-awards styling, fake grain everywhere, or cinema-ticket motifs.

The experience should be premium, calm, readable, and fast.

## Non-goals

Do not build:

- separate Generation controls or workflow
- publishing controls
- social scheduling
- visual generation
- poster generation
- analytics dashboards
- token-spend dashboards
- user management
- elaborate catalog charts
- autonomous background generation

The dashboard should make the creative operating system easier to steer and inspect, not become a second operating system.
