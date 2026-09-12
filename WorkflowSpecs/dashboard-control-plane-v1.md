# Never Coming Soon
## Internal Dashboard Control Plane Specification v1.0

## 1. Objective

Build a lightweight internal dashboard that gives the editor one place to steer Ideation, review the creative queue, launch Generation, add notes, and open finished drafts.

The dashboard is a control surface. It is not a new creative engine and it does not duplicate the logic already owned by n8n, GitHub governance, Google Sheets, or Google Drive.

## 2. Architecture

Recommended stack:

- Next.js App Router
- TypeScript
- Tailwind CSS or equivalent utility styling
- server-side route handlers for all n8n calls
- no LLM, Google, or n8n credentials exposed to the browser

Flow:

Dashboard UI -> server-side dashboard API -> authenticated n8n endpoints -> existing NCS workflows / Sheets / Drive

GitHub remains the source of truth for creative governance and workflow contracts.

## 3. Primary dashboard sections

### A. Explore

Large search-like creative input at the top of the page.

The editor may enter anything from one word to a developed brief.

Visible controls:

- direction input
- format: Any / Film / Series / Limited Series
- genre: Any or a selected / typed genre
- primary action: `Explore`

Advanced controls, collapsed by default:

- Approach: Auto or one of the ten canonical Ideation modes
- Anchor: Loose / Centered / Strict

Directed runs use `run_type = DIRECTED`.

Recommended directed seed count: 12 unless changed later by product configuration.

### B. General Ideation

A clear `Run General Ideation` action.

No creative query required.

Use `run_type = GENERAL` and the normal 30-seed default.

### C. Quick Sparks

Show five disposable one-sentence concepts on dashboard load.

Quick Sparks are generated through the dedicated Dashboard prompt and schema, not the full Ideation workflow.

Rules:

- not persisted
- no idea IDs
- no catalog-memory impact
- may use a cheaper model
- cache for the current browser session or a short TTL so refresh does not repeatedly spend tokens
- provide `Give me 5 more`
- each card offers `Develop this`

`Develop this` sends the exact spark into `run_type = CONCEPT_INTAKE`.

### D. Ideas Queue

Show high-level concept information from the `Ideas` tab.

Minimum visible fields:

- `idea_id`
- working title
- format
- genre
- status
- premise

Useful actions:

- edit / save `human_notes`
- Generate for eligible `DEVELOPMENT_SELECT` rows
- copy ID

Do not expose internal JSON by default.

A compact filter/search field for the existing queue is useful but should remain visually secondary to the Explore creative input.

### E. Generation Controls

Provide:

- `Generate Next` -> Generation with blank `idea_id`
- `Generate` on a specific eligible idea -> Generation with that `idea_id`
- advanced `Redevelop` only behind confirmation -> explicit `idea_id` plus `force_redevelopment = true`

Do not expose raw model creativity, temperature, randomness, or reasoning controls.

### F. Productions

Show high-level state from the `Productions` tab.

Minimum visible fields:

- `production_id`
- `idea_id`
- current / final title when available
- status
- latest overall editorial score when available
- final draft availability

Provide `Open Draft` when a Drive document URL or file identifier is available through the dashboard API.

Do not make the dashboard a publishing surface.

## 4. Workflow status

The dashboard should show a small unobtrusive current-operation area.

Examples:

- `Ideation running`
- `Generation NCS-I-000214 · CANON_READY`
- `Ready for human review`
- `Last run failed`

Do not require real-time sockets in v1. Polling is sufficient.

## 5. Ideation request contract

Dashboard-directed Ideation should map to `Schemas/ideation-run-request.schema.json`.

Canonical fields:

- `run_type`
- `target_seed_count`
- `human_direction`
- `concept_text`
- `target_format`
- `target_genre`
- `preferred_ideation_mode`
- `anchor_strength`
- `human_notes`

Behavior is governed by `Governance/ncs-directed-ideation-standard.md`.

## 6. Generation request contract

Generation remains intentionally simple.

Inputs:

- `idea_id`: optional
- `force_redevelopment`: optional boolean, default false

Blank `idea_id` means first eligible Development Select.

## 7. Dashboard server API

The frontend should call local server routes rather than n8n directly.

Recommended internal routes:

- `GET /api/overview`
- `POST /api/sparks`
- `POST /api/ideation/general`
- `POST /api/ideation/directed`
- `POST /api/ideation/intake`
- `POST /api/generation`
- `PATCH /api/ideas/:ideaId/notes`
- `GET /api/runs/:executionId` when n8n execution polling is available

Implementation may adapt names, but keep a clean adapter layer such as `lib/n8n.ts` so webhook details do not leak into UI components.

## 8. Environment configuration

Use server-only environment variables for n8n endpoints / credentials.

Suggested names:

- `N8N_IDEATION_WEBHOOK_URL`
- `N8N_GENERATION_WEBHOOK_URL`
- `N8N_QUICK_SPARKS_WEBHOOK_URL`
- `N8N_OVERVIEW_WEBHOOK_URL`
- `N8N_IDEA_NOTES_WEBHOOK_URL`
- `N8N_API_TOKEN` or equivalent shared secret if required

Do not expose these through `NEXT_PUBLIC_` variables.

If endpoints are not ready during initial dashboard construction, implement a mock adapter behind the same interfaces and make switching to live n8n configuration an environment-only change.

## 9. Quick Sparks client behavior

On first dashboard load in a browser session:

1. check session cache
2. if five current sparks exist, render them
3. otherwise call `/api/sparks`
4. store returned sparks in session storage or equivalent short-lived client cache

`Give me 5 more` explicitly requests a fresh batch and replaces or appends to the current batch.

No spark should reach Google Sheets until `Develop this` is clicked.

## 10. Ideas and Productions data

The dashboard should consume normalized API objects, not parse spreadsheet JSON cells in React components.

The server / n8n adapter should return only the fields the UI needs.

Recommended Idea summary:

```json
{
  "idea_id": "NCS-I-000214",
  "working_title": "The Row",
  "format": "FILM",
  "genre": "Romance",
  "status": "DEVELOPMENT_SELECT",
  "premise": "...",
  "human_notes": "...",
  "can_generate": true
}
```

Recommended Production summary:

```json
{
  "production_id": "NCS-P-000214",
  "idea_id": "NCS-I-000214",
  "title": "The Row",
  "status": "READY_FOR_HUMAN_REVIEW",
  "overall_score": 8.7,
  "draft_url": "..."
}
```

## 11. Human notes

Human notes are first-class creative input.

The dashboard must support editing and saving `human_notes` on an Idea without overwriting any machine-owned field.

Notes should save explicitly rather than on every keystroke in v1.

## 12. Safety around irreversible actions

No delete controls are required in dashboard v1.

`force_redevelopment` must require explicit confirmation because it invalidates machine-owned downstream Generation state.

The dashboard must never create a PUBLISH action.

## 13. Visual standard

Follow `Governance/ncs-dashboard-visual-standard.md`.

The page should feel like an editorial studio console rather than a dense enterprise dashboard.

## 14. V1 non-goals

Do not build in v1:

- publication or newsletter sending
- visual/poster/carousel generation
- social scheduling
- token / cost analytics
- complex charts
- arbitrary LLM parameter sliders
- multi-user permissions
- drag-and-drop workflow construction
- direct Google Sheets editing outside explicit human-note actions

## 15. Success test

The editor should be able to open one page and, without touching n8n or Google Sheets directly:

- get five fresh sparks
- explore a creative territory with a short query and optional format / genre constraints
- run broad Ideation
- inspect Development Select concepts
- add a human note
- generate the next eligible production
- generate a specific selected production
- see Generation progress at a useful high level
- open a finished draft

Anything that does not support those actions should justify its complexity.