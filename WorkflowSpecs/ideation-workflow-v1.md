# Never Coming Soon
## n8n Ideation Workflow Specification v1.1

## 1. Objective

Build one n8n workflow that turns a request for a new ideation run into a persistent field of Never Coming Soon concepts and one or more concepts worth handing to Generation.

The workflow does not draft full editions.

Its terminal creative output is `development_packet_json` written back to selected rows in the same Google Sheet.

Development Select is not an artificial scarcity gate. Select every concept that genuinely deserves a Generation attempt.

## 2. External systems

- GitHub: source of truth for governance, prompts, schemas, and this workflow specification
- Google Sheets: persistent creative memory and state
- LLM provider: creative and editorial judgment
- n8n Code nodes: deterministic IDs, hashing, normalization, compact retrieval, JSON handling, and routing

## 3. Google Sheet contract

Use one native Google Sheet tab named `Ideas`.

The live Sheet must contain exactly the canonical 21 columns defined in `Governance/ncs-data-contract.md`.

Do not create additional tabs or columns in v1.1.

The workflow must address rows by `idea_id`, never by visible row number after sorting or filtering.

Structured JSON cells are stored as compact JSON text and parsed exactly once when read.

## 4. Workflow inputs

Support at minimum a manual trigger.

Input parameters:

- `target_seed_count`, default `30`
- `human_direction`, optional free text

Optional later triggers may schedule runs, but scheduling is not required for v1.1.

Run Ideation executions serially in v1.1, or use an equivalent lock around ID reservation. Two simultaneous runs must never reserve the same `idea_id` block.

Do not introduce a default Development Select quota.

## 5. Model behavior

Use the strongest available model class for all creative and editorial nodes.

Do not save cost by routing the Seed Generator, Idea Expander, or Development Selector to a materially weaker model.

Behavior should differ by node:

- Ideation Director: strong reasoning, moderate creative variance
- Seed Generator: highest creative variance of the workflow while preserving coherence
- Duplicate Auditor: low variance, high consistency
- Concept Curator: low-to-moderate variance, strong editorial judgment
- Idea Expander: high creative capability with controlled optionality
- Development Selector: low-to-moderate variance, strong editorial judgment

Provider-specific temperature or reasoning settings may be chosen by the builder. The goal is stable judgment for evaluators and broader exploration for creators.

## 6. Prompt assembly contract

This is mandatory.

A prompt file that names a governance path does not have access to that file merely because the path appears in text.

For every LLM node, construct the system message from:

1. the full text of the node's `.system.md` prompt
2. the full current text of every governance document listed under that prompt's `AUTHORITATIVE GOVERNANCE`

Use clear delimiters around each injected governance document.

Example shape:

```text
[AGENT SYSTEM PROMPT]
...full system prompt...

[GOVERNANCE: ncs-brand-constitution.md]
...full document...

[GOVERNANCE: ncs-ideation-constitution.md]
...full document...
```

Then render the matching `.user.md` file with runtime variables as the user message.

Pass the matching JSON Schema to the provider's structured-output mechanism when supported.

Do not rely on file paths alone.

Load governance and prompt files once per run when practical and reuse the contents downstream.

## 7. Context hygiene

More context is not automatically better.

The Director may see broad catalog summaries and a compact memory digest because it needs to detect drift and design the creative session.

The Seed Generator should not receive individual historical NCS concepts by default. Give it only:

- the assigned ideation mode
- the Director's open-ended provocation
- the Director's creative permission
- human direction

This is intentional. Showing a creator the concepts it is supposed not to copy can anchor generation around those same concepts. Duplicate control belongs downstream.

The Duplicate Auditor should receive the broadest historical memory that safely fits the context budget because comparison is its job.

The Concept Curator should judge the nonduplicate seeds on creative potential, not on slate balancing.

The Idea Expander should receive the concept being expanded, its score, its own duplicate note, and any human notes attached to that exact row. It should not receive unrelated catalog concepts.

The Development Selector may receive compact slate context because it decides what deserves Generation attention now, but slate context may not override strong creative quality.

This separation protects originality and reduces accidental imitation of prior NCS work.

## 8. Node sequence

### Node 1: Trigger

Receive:

- `target_seed_count`
- `human_direction`

Validate `target_seed_count >= 1`.

### Node 2: Load GitHub source of truth

Fetch current contents of:

#### Governance used by Ideation

- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-ideation-constitution.md`
- `Governance/ncs-catalog-memory-standard.md`
- `Governance/ncs-data-contract.md`

#### Ideation prompts

- all files under `Prompts/Ideation/`

#### Schemas

- `Schemas/ideation-director.schema.json`
- `Schemas/seed-batch.schema.json`
- `Schemas/duplicate-audit.schema.json`
- `Schemas/concept-curation.schema.json`
- `Schemas/expanded-concept.schema.json`
- `Schemas/development-select.schema.json`

Fail the run if required source-of-truth files cannot be loaded.

Do not silently substitute hard-coded stale prompt text.

### Node 3: Read Ideas sheet

Read existing rows from `Ideas`.

At minimum ingest:

- `idea_id`
- `created_at`
- `status`
- `ideation_mode`
- `working_title`
- `format`
- `genre`
- `creative_tags_json`
- `premise`
- `creative_kernel`
- `fingerprint`
- `duplicate_check_json`
- `concept_score_json`
- `human_notes`

Parse structured cells only when nonblank.

### Node 4: Build catalog context

Use deterministic Code logic to calculate obvious counts and compact summaries, including:

- recent format mix
- recent primary genre mix
- recent ideation mode usage
- common arenas, settings, tones, or story engines when parseable
- counts by ideation status

Weight `DEVELOPMENT_SELECT` and `DEVELOP` more heavily when describing creative drift.

`PROMISING` may inform context.

`HOLD` should have less influence on slate drift.

Do not attempt to infer subtle creative meaning with spreadsheet arithmetic.

### Node 5: Build catalog memory digest

Create a compact historical memory object for the Ideation Director and duplicate-audit stages.

For each prior nonduplicate row, the compact representation should normally include only:

- `idea_id`
- `status`
- `premise`
- `creative_kernel`
- `creative_tags_json.memory_signature` when present
- a short human-note signal only when the note materially affects repetition or reuse

Do not pass long historical pitches.

When this compressed catalog fits comfortably inside the model context budget, preserve complete catalog coverage for duplicate auditing.

When it no longer fits comfortably, maintain two views:

1. a compact Director digest emphasizing recent and creatively important rows for drift awareness
2. a duplicate-audit memory assembled through staged retrieval or chunking so semantically similar concepts are not missed merely because they use different vocabulary

The Seed Generator does not receive this digest by default.

### Node 6: Ideation Director

Use:

- `Prompts/Ideation/01-ideation-director.system.md`
- `Prompts/Ideation/01-ideation-director.user.md`
- `Schemas/ideation-director.schema.json`

Inputs:

- target seed count
- compact catalog context
- catalog memory digest
- human direction

Validate:

- output matches schema
- `target_seed_count` echoes the requested value
- mode-plan counts sum exactly to target seed count

If validation fails, run one structure-only repair attempt using the validation error.

### Node 7: Split mode plan

Create one n8n item per Director room.

Each item contains:

- `ideation_mode`
- `count`
- `provocation`
- `creative_permission`
- human direction

Do not attach individual historical concepts or the catalog memory digest to Seed Generator items.

### Node 8: Seed Generator, parallel and independent

Run one Seed Generator call per room.

Use:

- `Prompts/Ideation/02-seed-generator.system.md`
- `Prompts/Ideation/02-seed-generator.user.md`
- `Schemas/seed-batch.schema.json`

The parallel rooms must not see one another's outputs before generation. Independence is intentional and increases exploration.

Validate that each room returns exactly its requested seed count.

If a room returns the wrong count or schema-invalid output, retry once with a repair instruction that preserves creative content where possible.

### Node 9: Merge seed batches

Flatten all returned seeds into individual candidate items.

Preserve:

- originating `ideation_mode`
- stable generation order within the run
- seed-level `initial_possibilities` in n8n execution data for Curator and Expander use

Validate total candidate count equals `target_seed_count` before continuing.

### Node 10: Reserve idea IDs and timestamps

Read all existing `idea_id` values.

Parse the maximum numeric suffix from IDs matching `NCS-I-######`.

Reserve the next contiguous block for the entire candidate batch before parallel downstream work.

Example:

Existing max: `NCS-I-000184`

30 new candidates receive `NCS-I-000185` through `NCS-I-000214`.

Assign one `created_at` ISO 8601 timestamp per candidate.

Initial status is `RAW`.

Do not use Sheet row count as the ID source.

### Node 11: Canonicalize memory signatures and generate fingerprints

For each candidate:

1. take `normalized_signature`
2. normalize strings to lowercase
3. trim whitespace
4. collapse repeated internal whitespace
5. normalize empty values to null
6. serialize using stable key ordering
7. generate SHA-256 in a Code node

The LLM must never generate the hash.

Merge the canonical normalized signature into `creative_tags_json` under `memory_signature`.

Write the hash to `fingerprint`.

### Node 12: Persist every raw candidate

Write every generated seed to the `Ideas` Sheet before semantic auditing or curation.

Populate:

- `idea_id`
- `created_at`
- `status = RAW`
- `ideation_mode`
- `working_title`
- `format`
- `genre`
- `creative_tags_json`
- `premise`
- `creative_kernel`
- `why_exciting`
- `fingerprint`

Leave later-stage fields blank.

`initial_possibilities` remains in n8n execution data rather than adding another Sheet column.

Persisting here ensures a later workflow failure does not erase the generated creative inventory.

### Node 13: Assemble historical memory for seed-stage duplicate audit

Build the broadest compact historical comparison set that safely fits the model context budget.

If the compressed prior catalog fits comfortably, pass the complete nonduplicate catalog memory rather than relying on narrow keyword retrieval.

If the catalog is too large, use staged retrieval or chunking. Retrieval signals may include:

- exact fingerprint match
- same or related genre
- same arena
- fields inside `memory_signature`
- premise and creative-kernel language
- status and recency
- explicit human notes about resemblance

Do not use SHA hash distance as similarity.

Do not rely on token overlap alone. Two concepts can be dramatic reskins while using different nouns.

Always include exact fingerprint collisions and strong canonical memory such as relevant `DEVELOPMENT_SELECT` rows.

The exact retrieval or chunking implementation can evolve without changing the Sheet contract.

### Node 14: Seed-stage Duplicate Auditor, batch

Use:

- `Prompts/Ideation/03-duplicate-auditor.system.md`
- `Prompts/Ideation/03-duplicate-auditor.user.md`
- `Schemas/duplicate-audit.schema.json`

Inputs:

- entire current candidate batch
- historical memory for comparison, complete or staged/chunked depending on catalog size
- exact fingerprint collisions

The auditor compares:

- each candidate to the supplied historical memory
- current candidates to one another

Validate exactly one audit result per candidate `idea_id`.

### Node 15: Write seed-stage duplicate results

For every candidate:

- write compact audit JSON to `duplicate_check_json`
- if audit status is `DUPLICATE`, set Sheet status to `DUPLICATE`
- if audit status is `CLEAR` or `OVERLAP`, keep status `RAW`

Duplicates remain persisted but do not enter curation.

If every current candidate is `DUPLICATE`, skip curation and downstream creative nodes and finish with the operational summary.

### Node 16: Concept Curator

Batch all nonduplicate candidates.

Use:

- `Prompts/Ideation/04-concept-curator.system.md`
- `Prompts/Ideation/04-concept-curator.user.md`
- `Schemas/concept-curation.schema.json`

Inputs:

- nonduplicate candidate batch, including seed-level `initial_possibilities`
- human direction

Validate exactly one result per input `idea_id`.

Also validate scorecard/status consistency:

- `DEVELOP` pairs with `would_develop = YES`
- `PROMISING` pairs with `YES` or `MAYBE`
- `HOLD` pairs with `MAYBE` or `NO`

Repair structure or enum inconsistency once if needed. Do not rerun creative judgment merely to change a score.

### Node 17: Write curation state

For every curated candidate:

- write `concept_score_json` from the Curator's `scorecard`
- set `status` to `DEVELOP`, `PROMISING`, or `HOLD`

Do not impose a fixed number of `DEVELOP` concepts.

### Node 18: Filter DEVELOP

Only rows marked `DEVELOP` enter Idea Expansion during the current run.

`PROMISING` and `HOLD` remain persisted for future use.

If no concepts are marked `DEVELOP`, skip expansion, the second duplicate audit, and Development Selection. Finish with the operational summary.

### Node 19: Idea Expander, parallel

Run one call per `DEVELOP` concept.

Use:

- `Prompts/Ideation/05-idea-expander.system.md`
- `Prompts/Ideation/05-idea-expander.user.md`
- `Schemas/expanded-concept.schema.json`

Inputs:

- full seed object, including `initial_possibilities`
- concept score
- seed-stage duplicate context
- any `human_notes` on that exact row

Do not expose unrelated human notes from other rows.

### Node 20: Write expanded fields and refresh memory signature

For each expanded concept, write back:

- `working_title`
- `format`
- `genre`
- `premise`
- `creative_kernel`
- `why_exciting`
- `short_pitch`
- `characters_json`
- `story_core_json`
- `signature_scenes_json`
- `interrogation_json`

Then:

1. canonicalize the Expander's new `normalized_signature` using the same rules as Node 11
2. merge it into `creative_tags_json.memory_signature`
3. preserve the Expander's other `creative_tags` values
4. regenerate SHA-256
5. overwrite `fingerprint`

Do not overwrite `human_notes`.

This refresh is mandatory because expansion may materially change the idea.

### Node 21: Assemble expanded-stage historical memory

Repeat the same quality-first memory strategy used at Node 13, now using the richer expanded concept and refreshed signature.

When complete compressed catalog coverage fits, use it. Otherwise use staged retrieval or chunking without relying on vocabulary alone.

Also include the other expanded concepts from the current run for same-run comparison.

### Node 22: Expanded-stage Duplicate Auditor, batch

Reuse:

- `Prompts/Ideation/03-duplicate-auditor.system.md`
- `Prompts/Ideation/03-duplicate-auditor.user.md`
- `Schemas/duplicate-audit.schema.json`

Inputs:

- expanded current-run candidate batch
- expanded-stage historical memory
- refreshed exact fingerprint collisions

Validate exactly one result per expanded `idea_id`.

### Node 23: Write expanded-stage duplicate results

For every expanded concept:

- overwrite `duplicate_check_json` with the latest audit
- if status is `DUPLICATE`, set Sheet status to `DUPLICATE`
- if status is `CLEAR` or `OVERLAP`, keep status `DEVELOP` for Development Selection

If every expanded concept is now `DUPLICATE`, skip Development Selection and finish with the operational summary.

### Node 24: Development Selector

Batch every expanded nonduplicate concept.

Use:

- `Prompts/Ideation/06-development-selector.system.md`
- `Prompts/Ideation/06-development-selector.user.md`
- `Schemas/development-select.schema.json`

Inputs:

- expanded concepts
- latest duplicate audits
- compact catalog context
- a map of nonblank `human_notes` for only these candidate IDs
- human direction

There is no default count target.

The Selector may return any number of `DEVELOPMENT_SELECT` decisions supported by the quality of the batch.

### Node 25: Write final ideation state

For each Development Selector decision:

- set status to `DEVELOPMENT_SELECT`, `PROMISING`, or `HOLD`
- if `DEVELOPMENT_SELECT`, write `development_packet_json`
- if not selected, leave `development_packet_json` blank

Never overwrite a pre-existing nonblank Development Packet without an explicit re-development action.

### Node 26: End summary

Return a concise execution summary containing:

- total seeds generated
- seed-stage duplicate count
- curator DEVELOP count
- curator PROMISING count
- curator HOLD count
- expanded-stage duplicate count
- final DEVELOPMENT_SELECT count
- final PROMISING count among expanded concepts
- final HOLD count among expanded concepts
- selected idea IDs and working titles
- any schema repair attempts
- any item-level errors

The summary is operational. It is not another creative review stage.

## 9. Structured-output repair

For any LLM node returning invalid JSON or schema-invalid output:

1. retry once using the original response plus the exact validation error
2. instruct the same model to repair structure only
3. do not invite substantive creative rewriting unless a required field is genuinely missing

If the repair fails:

- record the item-level error
- continue the run when downstream logic can safely proceed
- fail the run only when the missing output blocks the entire batch or would corrupt state

## 10. Google Sheets write rules

- use `idea_id` as the update key
- preserve `created_at`
- never erase `human_notes`
- stringify JSON exactly once
- leave not-yet-produced structured cells blank
- use retry with exponential backoff for transient write failures
- do not regenerate creative content merely because persistence failed

## 11. Human authority

`human_notes` is human-owned.

Human direction supplied at trigger time has high authority for that run.

Human notes can override later duplicate or selection judgments through an explicit future re-development action.

The workflow should assist judgment, not create irreversible editorial law.

## 12. No hidden killing

Do not silently drop concepts.

Every generated seed should end the run persisted with an explicit status whenever technically possible.

Development Select is not proof that the final production will publish.

Generation may discover a better version or fail to make the concept work. The human editor ultimately decides whether a fully developed production is good enough.

## 13. V1.1 non-goals

Do not build these unless later requested:

- vector database
- multi-tab Sheet architecture
- dedicated actor registry
- dedicated character registry
- autonomous research on every seed
- separate title agent
- separate casting agent
- automatic Generation workflow trigger
- automatic scheduled resurrection of HOLD ideas

V1.1 should remain legible, debuggable, and creatively focused.
