# Never Coming Soon
## Unified Workflow Entry Modes v2.0

This file defines the supported entry modes for the single Never Coming Soon n8n creative workflow.

All entry modes ultimately join the same downstream path:

`Ideation -> duplicate control -> curation -> expansion -> Development Select -> Production Builder -> Social Release Builder -> DRAFTED`

Development Select is no longer the normal terminal state.

Asset generation remains manual and outside n8n.

## GENERAL

Input:

- `target_seed_count`, default 30

Optional:

- `human_direction`
- `human_campaign_notes`

Run the standard broad Ideation path.

Every newly selected concept proceeds into production development and social packaging during the same workflow execution.

## DIRECTED

Inputs:

- `target_seed_count`, default 30
- `human_direction`, required
- `target_format`, default `ANY`
- `target_genre`, default `ANY`
- `preferred_ideation_mode`, default `AUTO`
- `anchor_strength`, default `CENTERED`
- `human_campaign_notes`, optional

Apply:

`Governance/ncs-directed-ideation-standard.md`

in addition to normal Ideation governance.

The Director and every Seed Generator room receive the same direction and constraints.

Concrete format and genre constraints must be honored.

If mode is AUTO, the Director may allocate several ideation modes. Otherwise use the selected mode.

After seed generation, continue through the full unified workflow.

## CONCEPT_INTAKE

Inputs:

- `concept_text`, required
- `target_format`, optional
- `target_genre`, optional
- `human_notes`, optional
- `human_campaign_notes`, optional

Run:

- `Prompts/Ideation/00-concept-intake.system.md`
- `Prompts/Ideation/00-concept-intake.user.md`
- `Schemas/concept-intake.schema.json`

Create one structured seed, assign the next durable idea ID, persist it as RAW, then join the normal workflow at duplicate audit.

Do not run the Director or normal batch Seed Generator for Concept Intake.

The concept still goes through:

- duplicate audit
- curation
- expansion when applicable
- second duplicate audit
- Development Selection

Human submission does not guarantee selection.

If selected, it immediately continues through Production Builder and Social Release Builder during the same execution.

## Optional debug control

The workflow may support:

`stop_after_development_select = true`

for testing or debugging.

Default must be false.

Do not use this as normal production behavior.

## Optional redevelopment control

The same workflow may later support an explicit existing-idea route such as:

- `idea_id`
- `force_redevelopment = true`

That route may begin from an existing nonblank Development Packet and skip seed-generation stages.

If implemented, it must preserve the last successful DRAFTED artifact until the replacement package fully succeeds.

## Canonical controls

Formats:

- ANY
- FILM
- SERIES
- LIMITED_SERIES

Modes:

- AUTO
- character_first
- relationship_first
- world_first
- situation_first
- genre_first
- scene_first
- ending_first
- truth_first
- discovery_first
- title_first

Anchor strength:

- LOOSE
- CENTERED
- STRICT

Genre remains free text with convenience presets in the dashboard.

## Invocation

Keep one callable workflow implementation.

Do not duplicate Ideation and Production Packaging into separate n8n workflows.

Return one compact operational result with:

- run type
- execution status
- created idea IDs
- counts by Ideation status
- selected concept IDs
- DRAFTED concept IDs
- production/package failures
- error summary
