# Never Coming Soon
## Internal Dashboard Control Plane v1.1

## Purpose

Build a lightweight internal control surface for managing Never Coming Soon Ideation and Generation. This is not a public publishing product and should not duplicate creative logic already owned by n8n and GitHub.

The dashboard is a thin client. n8n remains the orchestration layer. The Ideas Google Sheet remains persistent catalog and lifecycle state. Google Drive remains the final draft archive. GitHub remains the source of truth for creative governance, prompts, schemas, and workflow specs.

There is no separate Productions state layer in the current architecture.

## Primary dashboard actions

### Explore

The dashboard has one large creative-direction input. It may contain anything from one word to a detailed brief.

Optional controls:

- format: ANY, FILM, SERIES, LIMITED_SERIES
- genre: ANY or common/free-text genre
- ideation mode: AUTO or one of the canonical Ideation modes
- anchor strength: LOOSE, CENTERED, STRICT

Submitting Explore invokes Directed Ideation.

### Run General Ideation

One action with no creative direction. It invokes the normal broad Ideation workflow using the normal target seed count, default 30.

### Quick Sparks

When the dashboard loads, show five disposable one-sentence concepts. Generate a new batch only when no recent session batch is available, with a visible `Give me 5 more` action.

Quick Sparks are not written to Sheets, assigned idea IDs, or included in catalog memory.

Each Spark shows:

- one-sentence concept
- provisional format
- provisional genre
- `Develop this`
- `Dismiss`

`Develop this` invokes Concept Intake for that exact concept, not a broad Directed Ideation run.

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
- ncs_score when available
- draft_url when available
- published_url when available

Useful actions:

- edit human notes
- Send to Generation for a specific eligible idea
- human Development Select override when appropriate, visibly marked as a human action
- Open Draft when `draft_url` exists
- inspect or copy the social asset handoff when `ig_packet_json` exists

### Generation controls

Provide:

- `Generate Next`, which invokes Generation with blank idea_id
- `Generate This`, which invokes Generation with a selected idea_id
- advanced `Redevelop`, which requires explicit confirmation and invokes `force_redevelopment=true` for a specific idea

Generation lifecycle shown in the same Ideas row:

`DEVELOPMENT_SELECT` -> `GENERATING` -> `DRAFTED`

A later human or publishing workflow may set:

`DRAFTED` -> `PUBLISHED`

Do not create a dashboard-side status system.

### Drafted productions view

If a separate view is useful, derive it from Ideas rows where status is `DRAFTED` or `PUBLISHED`.

Display at minimum:

- idea_id
- final_title
- format
- genre
- status
- ncs_score
- draft_url
- published_url

This is a filtered view of Ideas, not another data source.

Show a compact in-progress state for rows currently marked `GENERATING`.

## Ideation entry contract

The dashboard-facing Ideation request contract is:

- run_type: GENERAL, DIRECTED, or CONCEPT_INTAKE
- target_seed_count: integer, normal default 30
- human_direction: optional string
- concept_text: required only for CONCEPT_INTAKE
- target_format: ANY, FILM, SERIES, or LIMITED_SERIES
- target_genre: optional string or ANY
- preferred_ideation_mode: AUTO or one canonical Ideation mode
- anchor_strength: LOOSE, CENTERED, or STRICT, default CENTERED
- human_notes: optional

GENERAL ignores creative filters except target_seed_count.

DIRECTED requires human_direction and uses the Directed Ideation Standard.

CONCEPT_INTAKE requires concept_text and creates one seed before joining the normal duplicate-audit, curation, expansion, and Development Selection pipeline.

## Generation entry contract

- idea_id: optional string
- force_redevelopment: optional boolean, default false

Blank idea_id means the first eligible `DEVELOPMENT_SELECT` row with a nonblank Development Packet.

## Security and architecture

Recommended application stack: Next.js, TypeScript, App Router, and a small server-side API layer.

The browser must not hold Google, OpenAI, Anthropic, GitHub, or n8n credentials.

Use:

Dashboard browser -> dashboard server routes -> authenticated n8n endpoints -> existing workflows and data systems.

Do not reimplement Ideation or Generation logic in the web app.

## Visual direction

The dashboard should feel like a clean internal studio console rather than SaaS analytics software.

House palette:

- Midnight Navy: #08192F
- Warm Ivory: #F5EEDF
- Reel Orange: #F24B2C
- Electric Cobalt: #315CFF
- Slate: #202632

Use Midnight Navy and Warm Ivory as the dominant surfaces. Reel Orange is the primary action/accent color. Electric Cobalt should be rare and functional.

Typography should pair a high-contrast editorial serif for large display moments with a neutral modern sans for controls, metadata, and tables. Use widely available web fonts rather than private font files. Recommended implementation: Cormorant Garamond or Libre Baskerville for selective display use, and Inter for the interface. Do not overuse the serif inside dense operational UI.

Avoid film-industry cliches such as clapperboards, film strips, projectors, red velvet, gold-awards styling, fake grain everywhere, or cinema-ticket motifs.

The experience should be premium, calm, readable, and fast.

## V1 non-goals

Do not build publishing controls, social scheduling, visual generation, poster generation, analytics dashboards, token-spend dashboards, user management, elaborate catalog charts, or autonomous background generation in v1.

The dashboard should make the existing creative operating system easier to steer and inspect, not become a second operating system.
