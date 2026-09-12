# Never Coming Soon
## Ideation Workflow Dashboard Amendment v1.0

This document amends `WorkflowSpecs/ideation-workflow-v1.md` for dashboard-driven entry modes. Where this amendment conflicts with the older trigger contract, this document controls.

## 1. New callable input contract

Ideation must support both the existing manual trigger and a callable n8n entry point for the dashboard.

Normalize all trigger sources into one request object matching `Schemas/ideation-run-request.schema.json`.

Supported run types:

- `GENERAL`
- `DIRECTED`
- `CONCEPT_INTAKE`

Defaults:

- GENERAL `target_seed_count = 30`
- DIRECTED `target_seed_count = 12`
- DIRECTED `anchor_strength = CENTERED`
- format, genre, and preferred mode are null unless supplied

## 2. GENERAL

GENERAL follows the existing v1.1 pipeline unchanged:

Director -> mode split -> independent Seed Generators -> persistence -> duplicate audit -> curation -> expansion -> second duplicate audit -> Development Selector.

No human direction is required.

## 3. DIRECTED

DIRECTED uses the same canonical pipeline as GENERAL but supplies the following request fields to the Ideation Director and every Seed Generator room:

- `human_direction`
- `target_format`
- `target_genre`
- `preferred_ideation_mode`
- `anchor_strength`

Load `Governance/ncs-directed-ideation-standard.md` for the Director and Seed Generator.

The Director still creates a mode plan. It must honor hard format / genre constraints and the requested anchor strength.

When `preferred_ideation_mode` is nonblank, the Director should materially weight the plan toward that mode according to the Directed Ideation Standard.

Seed Generator outputs must be deterministically validated against hard format constraints when one is supplied. If a seed violates a required format, use the existing single repair path rather than silently accepting it.

## 4. CONCEPT_INTAKE

CONCEPT_INTAKE is not a batch-generation run.

Sequence:

1. validate nonblank `concept_text`
2. load:
   - `Prompts/Ideation/00-concept-intake.system.md`
   - `Prompts/Ideation/00-concept-intake.user.md`
   - `Schemas/concept-intake.schema.json`
   - `Governance/ncs-directed-ideation-standard.md`
3. run one Concept Intake model call
4. validate / repair structure once using the normal structure-only repair rule
5. unwrap the returned `seed` into one canonical candidate
6. reserve one new `idea_id`
7. assign `created_at` and initial `RAW` status
8. canonicalize the normalized signature and generate the deterministic SHA-256 fingerprint
9. persist the raw candidate to the Ideas sheet
10. enter the existing seed-stage duplicate audit at the equivalent of Node 13
11. continue through Curator, expansion when marked DEVELOP, expanded-stage duplicate audit, and Development Selector exactly as normal

Do not run the Ideation Director or batch Seed Generator for CONCEPT_INTAKE.

## 5. Persistence

All canonical GENERAL, DIRECTED, and CONCEPT_INTAKE concepts use the same 21-column Ideas contract.

No new persistent Sheet fields are required for dashboard control values.

`ideation_mode` for CONCEPT_INTAKE should be stored as `concept_intake` even though that value is an orchestration provenance label rather than one of the ten Director creative modes. If the current Sheet validation prevents that value, update validation to permit it without changing the column contract.

Dashboard Quick Sparks never enter the Ideas sheet until a spark is explicitly sent through CONCEPT_INTAKE.

## 6. Human notes

Dashboard edits to `human_notes` remain human-owned and must never be overwritten by Ideation writes.

Concept Intake may receive optional human notes as input, but machine output must not replace the persistent human note cell.

## 7. Callable outputs

For dashboard-triggered runs, return a concise normalized response in addition to normal persistence behavior.

Recommended response:

```json
{
  "accepted": true,
  "run_type": "DIRECTED",
  "execution_id": "...",
  "target_seed_count": 12,
  "message": "Directed ideation started"
}
```

If the n8n webhook invocation remains synchronous, it may instead return final run summary data. Prefer asynchronous acceptance when the full run may take long enough to create browser timeout risk.

## 8. Quick Sparks

Quick Sparks should be a separate lightweight callable workflow or subworkflow. They should not invoke the canonical Ideation pipeline.

Load:

- `Prompts/Dashboard/01-quick-sparks.system.md`
- `Prompts/Dashboard/01-quick-sparks.user.md`
- `Schemas/quick-sparks.schema.json`

Recommended default count: 5.

Use a cheaper capable model than canonical Ideation. This is the deliberate exception to the Ideation rule requiring strongest models for creative judgment because Quick Sparks are disposable and not persisted.

Do not write to Google Sheets.

## 9. Existing duplicate and quality controls remain authoritative

Directed entry does not bypass:

- fingerprinting
- semantic duplicate auditing
- curation
- expansion rules
- Development Selection
- catalog memory

The dashboard changes how ideas enter the machine. It does not lower the quality gate.

## 10. Dashboard controls are intent controls

Do not add raw temperature, top-p, token budget, reasoning effort, or arbitrary creativity / randomness sliders to the callable contract.

Model configuration remains orchestration-owned.