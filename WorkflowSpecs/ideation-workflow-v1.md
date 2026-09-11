# Never Coming Soon
## n8n Ideation Workflow Specification v1.0

## 1. Objective

Build one n8n workflow that turns a request for a new ideation run into a persistent set of Never Coming Soon concepts and a small number of Development Selects.

The workflow does not draft full editions.

Its terminal output is one or more `development_packet_json` objects written back to the same Google Sheet rows.

## 2. External systems

- GitHub: source of truth for governance, prompts, schemas, and this workflow specification
- Google Sheets: persistent creative memory and state
- LLM provider: creative and editorial judgment
- n8n Code nodes: deterministic IDs, hashing, normalization, JSON handling, routing

## 3. Sheet contract

Use one Google Sheet tab named `Ideas`.

Canonical columns are defined in `Governance/ncs-data-contract.md`.

Do not create additional tabs unless a future version explicitly requires them.

## 4. Trigger

Support at minimum a manual trigger.

Optional later trigger: scheduled ideation runs.

Input parameters:

- `target_seed_count`, default 30
- `human_direction`, optional
- `develop_target_count`, optional soft target

## 5. Node sequence

### Node 1: Trigger

Receive run parameters.

### Node 2: Load governance from GitHub

Fetch the current contents of:

- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-ideation-constitution.md`
- `Governance/ncs-catalog-memory-standard.md`
- `Governance/ncs-data-contract.md`

Also fetch the relevant prompt and schema files for each downstream agent.

Prefer loading once per run and passing content forward rather than refetching repeatedly.

### Node 3: Read Ideas sheet

Read existing rows.

At minimum gather:

- idea_id
- created_at
- status
- ideation_mode
- working_title
- format
- genre
- creative_tags_json
- premise
- creative_kernel
- fingerprint
- duplicate_check_json
- human_notes

Recent concepts and prior structurally similar concepts are the most important context.

### Node 4: Build catalog context

Use deterministic n8n logic where possible to summarize:

- recent format mix
- recent genre mix
- ideation mode usage
- common arenas or settings
- recent statuses

Do not attempt to infer creative meaning mechanically beyond obvious counts.

### Node 5: Ideation Director

Use:

- `Prompts/Ideation/01-ideation-director.system.md`
- `Prompts/Ideation/01-ideation-director.user.md`

Inputs:

- target seed count
- catalog context
- relevant prior ideas
- human direction

Output:

- mode plan
- catalog observations
- creative permission

Validate that allocated counts sum to target seed count.

If not, retry once with a repair instruction.

### Node 6: Split mode plan

Create one item per mode plan row.

### Node 7: Seed Generator, parallel

Run the same Seed Generator agent independently for each mode.

Use:

- `Prompts/Ideation/02-seed-generator.system.md`
- `Prompts/Ideation/02-seed-generator.user.md`
- `Schemas/seed-batch.schema.json`

Inputs:

- ideation mode
- provocation
- requested count
- catalog observations
- relevant prior concepts
- human direction

Output one seed batch per mode.

### Node 8: Merge seed batches

Flatten all seeds into individual candidate items.

### Node 9: Assign IDs and timestamps

Deterministically assign:

- `idea_id`
- `created_at`
- initial `status = RAW`
- `ideation_mode`

ID strategy should be collision-safe and stable.

Recommended pattern if row counter is reliable:

`NCS-I-000001`

Alternative: short UUID-backed ID.

Do not reuse IDs from deleted rows.

### Node 10: Normalize structural signature

Take each seed's `normalized_signature` and canonicalize:

- lowercase
- trim whitespace
- sort array-like fields when order is irrelevant
- normalize nulls
- serialize with stable key ordering

### Node 11: Generate fingerprint

Use a Code node to create a deterministic SHA-256 hash of the canonical serialized signature.

The LLM must never generate the hash.

### Node 12: Candidate retrieval for duplicate audit

For each candidate, identify a manageable set of prior ideas for semantic comparison.

Initial v1 strategy may use:

- exact fingerprint matches
- same primary genre
- same arena
- same protagonist archetype or relationship keywords where available
- recent ideas

Do not feed the entire Sheet to every duplicate-audit call if the catalog becomes large.

### Node 13: Duplicate Auditor

Use:

- `Prompts/Ideation/03-duplicate-auditor.system.md`
- `Prompts/Ideation/03-duplicate-auditor.user.md`
- `Schemas/duplicate-audit.schema.json`

Write result to `duplicate_check_json`.

If status is `DUPLICATE`, set row status to `DUPLICATE` and do not send that concept to curation.

If `CLEAR` or `OVERLAP`, continue.

### Node 14: Write viable raw ideas to Google Sheets

Persist all candidates, including duplicates if desired for memory.

Recommended: persist duplicates too, with status `DUPLICATE`, so the system remembers rejected repetition.

Populate at minimum:

- identity fields
- seed fields
- fingerprint
- duplicate check

Persist early so a partial workflow failure does not lose the creative inventory.

### Node 15: Concept Curator

Batch all nonduplicate candidates.

Use:

- `Prompts/Ideation/04-concept-curator.system.md`
- `Prompts/Ideation/04-concept-curator.user.md`

Return scorecard and status for each idea.

Write:

- `concept_score_json`
- `status`

### Node 16: Filter DEVELOP

Only `DEVELOP` concepts enter the Idea Expander in the current run.

`PROMISING` and `HOLD` remain in the Sheet for future retrieval.

### Node 17: Idea Expander, parallel

Use:

- `Prompts/Ideation/05-idea-expander.system.md`
- `Prompts/Ideation/05-idea-expander.user.md`
- `Schemas/expanded-concept.schema.json`

For each `DEVELOP` concept, return expanded fields.

Write back:

- working_title if improved
- format if improved
- genre if improved
- creative_tags_json
- premise
- creative_kernel
- why_exciting
- short_pitch
- characters_json
- story_core_json
- signature_scenes_json
- interrogation_json

Do not overwrite `human_notes`.

### Node 18: Development Selector

Batch the expanded concepts.

Use:

- `Prompts/Ideation/06-development-selector.system.md`
- `Prompts/Ideation/06-development-selector.user.md`
- `Schemas/development-select.schema.json`

Inputs:

- expanded concepts
- recent catalog context
- human direction

Return a status and rationale for each.

### Node 19: Write final ideation state

For each selected row:

- set status to `DEVELOPMENT_SELECT`, `PROMISING`, or `HOLD`
- if `DEVELOPMENT_SELECT`, write `development_packet_json`

### Node 20: End summary

Return a concise workflow result containing:

- total seeds generated
- duplicate count
- DEVELOP count
- PROMISING count
- HOLD count
- DEVELOPMENT_SELECT count
- selected idea IDs and titles
- any node errors or schema repairs

## 6. Error handling

### Structured output repair

If an agent returns invalid JSON or schema-invalid output:

1. retry once with the validation error
2. instruct the same agent to repair structure only
3. do not invite substantive creative rewriting unless required by missing fields

If the second attempt fails, log the item and continue the run where possible.

### Google Sheets write failure

Retry with exponential backoff.

Do not regenerate creative content merely because persistence failed.

### GitHub fetch failure

Fail the run rather than silently using stale hard-coded governance, unless an explicit cached-governance strategy is added in a later version.

## 7. Human notes

`human_notes` is human-owned.

Agents may read it when relevant but must never erase or overwrite it.

## 8. No hidden killing

Do not drop concepts silently.

Every seed should end the run in a persisted status whenever technically possible.

## 9. Future extensions, not v1 requirements

Do not build these unless later requested:

- vector database
- multi-tab Sheet architecture
- dedicated actor registry
- dedicated character registry
- autonomous research on every seed
- separate title agent
- separate casting agent
- scheduled re-evaluation of HOLD ideas
- automatic Generation workflow trigger

V1 should remain legible and easy to debug.
