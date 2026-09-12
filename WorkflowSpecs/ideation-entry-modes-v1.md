# Never Coming Soon
## Ideation Entry Modes v1.0

This file extends the existing Ideation workflow with three dashboard entry modes while preserving the current duplicate-audit, curation, expansion, and Development Selection pipeline.

## GENERAL

Use the current broad Ideation behavior. Input: `target_seed_count`, default 30.

## DIRECTED

Inputs:

- `target_seed_count`, default 30
- `human_direction`, required
- `target_format`, default `ANY`
- `target_genre`, default `ANY`
- `preferred_ideation_mode`, default `AUTO`
- `anchor_strength`, default `CENTERED`

Apply `Governance/ncs-directed-ideation-standard.md` in addition to normal Ideation governance.

The Director and every Seed Generator room receive the same direction and constraints. Concrete format and genre constraints must be honored. If mode is AUTO, the Director may allocate several modes. Otherwise use the selected mode.

After seed generation, use the existing persistence and evaluation pipeline unchanged.

## CONCEPT_INTAKE

Inputs:

- `concept_text`, required
- `target_format`, optional
- `target_genre`, optional
- `human_notes`, optional

Run `Prompts/Ideation/00-concept-intake.system.md` and `.user.md` with `Schemas/concept-intake.schema.json`.

Create one structured seed, assign the next durable idea ID, persist it as RAW, then join the normal workflow at duplicate audit. Do not run the Director or normal batch Seed Generator for Concept Intake.

The concept still goes through normal curation, expansion when applicable, duplicate checking, and Development Selection. Human submission does not guarantee selection.

## Canonical controls

Formats: ANY, FILM, SERIES, LIMITED_SERIES.

Modes: AUTO, character_first, relationship_first, world_first, situation_first, genre_first, scene_first, ending_first, truth_first, discovery_first, title_first.

Anchor strength: LOOSE, CENTERED, STRICT.

Genre remains free text with convenience presets in the dashboard.

## Invocation

Keep the existing manual trigger. Add a callable entry point for the dashboard rather than copying the creative workflow into a separate implementation.

Return a compact operational result with run type, status, created idea IDs, count by final status, and any error summary.
