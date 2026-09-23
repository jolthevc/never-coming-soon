# Never Coming Soon
## Unified n8n Creative Workflow Specification v2.0

> Filename retained for loader compatibility.

## 1. Objective

Build one n8n workflow that takes Never Coming Soon from idea creation through a fully developed and socially packaged fictional production.

The workflow owns five phases:

1. Ideation
2. Duplicate control and curation
3. Development Select
4. Production development
5. Social release packaging

The workflow ends with selected productions in `DRAFTED` state, each with:

- a complete internal Canon Bible
- a durable human-readable production treatment in Google Drive
- a schema-valid `ncs_ig_v2` packet in `ig_packet_json`

There is no separate required Generation workflow.

Asset creation is manual and happens outside n8n.

Long-form article writing is optional later work and is not part of the normal path.

## 2. Core cost principle

Use model calls where they create material creative value.

Normal path for a concept that survives all Ideation gates:

- Ideation Director
- Seed Generator
- Duplicate Auditor
- Concept Curator
- Idea Expander
- second Duplicate Auditor
- Development Selector
- Production Builder
- Social Release Builder

Once a concept reaches Development Select, there are only two additional creative calls:

1. Production Builder
2. Social Release Builder

Do not run Casting Director, Edition Architect, Edition Writer, Forensic Editor, Revision Writer, scoring loops, or article polish merely to create the social product.

## 3. External systems

- GitHub: source of truth for governance, prompts, schemas, and workflow specifications
- Google Sheets: persistent catalog and lifecycle state
- Google Drive: durable production-treatment store
- LLM provider: creative and editorial judgment
- n8n Code nodes: deterministic IDs, hashing, normalization, validation, rendering, routing, and summaries

## 4. Google Sheet contract

Use one native Google Sheet tab named `Ideas`.

The Sheet contract is defined by:

`Governance/ncs-data-contract.md`

Use the canonical 26 columns in that document.

Address rows by `idea_id`, never by visible row number after sorting or filtering.

Structured JSON cells are stored as compact JSON text and parsed exactly once when read.

Do not add durable columns for intermediate agent outputs.

## 5. Workflow inputs

Support the entry modes defined in:

`WorkflowSpecs/ideation-entry-modes-v1.md`

At minimum the workflow must support:

- GENERAL
- DIRECTED
- CONCEPT_INTAKE

Common inputs may include:

- `target_seed_count`
- `human_direction`
- `human_notes`
- `human_campaign_notes`
- optional explicit research questions

Run ID reservation safely so concurrent runs cannot claim the same `idea_id`.

Do not impose a default Development Select quota.

## 6. Source snapshot

At the beginning of every execution:

1. resolve current GitHub `main` commit SHA
2. store it as `source_commit_sha`
3. load every required prompt, governance document, schema, and workflow contract from that same snapshot

Do not mix source versions inside one execution.

Do not silently fall back to stale hard-coded prompt text when GitHub loading fails.

## 7. Prompt assembly contract

A prompt file that references a governance filename does not automatically have access to that governance.

For each model call:

1. load the full system prompt
2. load every governance file required by that stage
3. concatenate them with clear document delimiters
4. render the matching user prompt with runtime variables
5. supply the matching JSON Schema through structured output when supported

Load stable source files once per execution and reuse them where practical.

## 8. Context hygiene

More context is not automatically better.

### Ideation Director

May receive:

- target seed count
- compact catalog context
- catalog memory digest
- human direction

### Seed Generator

Should not receive individual historical ideas by default.

Give it only:

- assigned ideation mode
- Director provocation
- creative permission
- human direction

Duplicate control belongs downstream.

### Duplicate Auditor

Receives the broadest historical memory safely available because comparison is its job.

### Concept Curator

Judges surviving seeds on creative potential.

### Idea Expander

Receives only the concept being expanded, its score, its own duplicate note, and relevant human notes.

### Development Selector

May receive compact slate context, but slate balance must not override strong creative quality.

### Production Builder

Receives only:

- the selected Development Packet
- relevant human notes
- optional research packet when one exists

Do not pass old article drafts, old scores, broad catalog history, unrelated concepts, or social copy.

### Social Release Builder

Receives only:

- final Canon Bible
- optional human campaign notes
- current visual/social/brand governance

Do not require an article or editorial score.

## 9. Phase A: Ideation

### Node 1: Trigger / entry router

Resolve run type and inputs.

For GENERAL and DIRECTED batch runs, validate `target_seed_count >= 1`.

For CONCEPT_INTAKE, require `concept_text`.

### Node 2: Load GitHub source of truth

Load all source files required by the active route.

At minimum, normal full-path executions need:

#### Governance

- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-ideation-constitution.md`
- `Governance/ncs-catalog-memory-standard.md`
- `Governance/ncs-data-contract.md`
- `Governance/ncs-story-development-standard.md`
- `Governance/ncs-publication-integrity-standard.md`
- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`
- relationship / television / research governance when relevant

#### Ideation prompts

- active files under `Prompts/Ideation/`

#### Production/package prompts

- `Prompts/Generation/01-production-builder.system.md`
- `Prompts/Generation/01-production-builder.user.md`
- `Prompts/Generation/10-ig-asset-packet-builder.system.md`
- `Prompts/Generation/10-ig-asset-packet-builder.user.md`

#### Schemas

- active Ideation schemas
- `Schemas/canon-bible.schema.json`
- `Schemas/ig-asset-packet.schema.json`

Fail when required source files cannot be loaded.

### Node 3: Read Ideas sheet

Read existing rows from `Ideas`.

At minimum ingest:

- idea identity and status
- working concept fields
- creative memory fields
- duplicate and score fields
- Development Packet
- human notes
- successful final fields when present

Parse structured cells only when nonblank.

### Node 4: Build catalog context

Use deterministic logic to calculate compact slate context such as:

- recent format mix
- recent primary genre mix
- ideation-mode usage
- common arenas, settings, tones, or engines when parseable
- counts by lifecycle status

Weight active and recently successful concepts more heavily for drift awareness.

### Node 5: Build catalog memory digest

Create compact historical memory for the Ideation Director and duplicate auditing.

For each prior nonduplicate concept, prefer:

- `idea_id`
- status
- premise
- creative kernel
- normalized memory signature
- only the human-note signal that materially affects repetition or reuse

Do not pass full historical pitches unless genuinely necessary.

### Node 6: Ideation Director

Use:

- `Prompts/Ideation/01-ideation-director.system.md`
- `Prompts/Ideation/01-ideation-director.user.md`
- `Schemas/ideation-director.schema.json`

Validate schema and count allocations.

Allow one structure-only repair.

### Node 7: Split mode plan

Create one item per Director room with:

- ideation mode
- requested count
- provocation
- creative permission
- human direction

### Node 8: Seed Generator

Run independent room calls.

Use:

- `Prompts/Ideation/02-seed-generator.system.md`
- `Prompts/Ideation/02-seed-generator.user.md`
- `Schemas/seed-batch.schema.json`

Rooms must not see one another's outputs before generation.

Validate exact requested counts.

Allow one structure-only repair.

### Node 9: Merge seeds

Flatten all rooms into candidate items.

Preserve originating ideation mode and generation order.

### Node 10: Reserve IDs and timestamps

Reserve one contiguous `NCS-I-######` ID block for the full candidate batch.

Do not use Sheet row count as the ID source.

### Node 11: Canonicalize memory signatures and hash

Normalize the seed memory signature deterministically and generate SHA-256 in Code.

The LLM never creates the fingerprint hash.

### Node 12: Persist RAW candidates

Write every generated seed to the Sheet before later evaluation.

Populate source fields only.

Keep `initial_possibilities` in execution memory rather than adding a new column.

## 10. Phase B: Duplicate control and curation

### Node 13: Seed-stage historical memory

Assemble the broadest compact comparison set that safely fits context.

When needed, use staged retrieval or chunking.

Do not use hash distance or vocabulary overlap as the sole similarity method.

### Node 14: Seed-stage Duplicate Auditor

Use:

- `Prompts/Ideation/03-duplicate-auditor.system.md`
- `Prompts/Ideation/03-duplicate-auditor.user.md`
- `Schemas/duplicate-audit.schema.json`

Compare candidates against historical memory and one another.

### Node 15: Write duplicate results

Persist the audit.

Set true duplicates to `DUPLICATE`.

Keep CLEAR and OVERLAP candidates eligible for curation.

### Node 16: Concept Curator

Use:

- `Prompts/Ideation/04-concept-curator.system.md`
- `Prompts/Ideation/04-concept-curator.user.md`
- `Schemas/concept-curation.schema.json`

Judge creative potential without imposing a slate quota.

### Node 17: Write curation state

Persist `concept_score_json`.

Set status to:

- `DEVELOP`
- `PROMISING`
- `HOLD`

### Node 18: Filter DEVELOP

Only `DEVELOP` candidates continue during the current execution.

### Node 19: Idea Expander

Use:

- `Prompts/Ideation/05-idea-expander.system.md`
- `Prompts/Ideation/05-idea-expander.user.md`
- `Schemas/expanded-concept.schema.json`

Run one call per DEVELOP concept.

### Node 20: Write expanded concept and refresh fingerprint

Persist expanded concept fields.

Rebuild normalized memory signature and deterministic fingerprint because expansion may materially change the concept.

### Node 21: Expanded-stage historical memory

Build the richer comparison set using expanded concepts.

Include other current-run expanded concepts for same-run comparison.

### Node 22: Expanded-stage Duplicate Auditor

Reuse Duplicate Auditor prompt and schema on expanded concepts.

### Node 23: Write expanded duplicate result

Set true duplicates to `DUPLICATE`.

Keep CLEAR and OVERLAP concepts at `DEVELOP` for Development Selection.

## 11. Phase C: Development Select

### Node 24: Development Selector

Use:

- `Prompts/Ideation/06-development-selector.system.md`
- `Prompts/Ideation/06-development-selector.user.md`
- `Schemas/development-select.schema.json`

Inputs:

- expanded surviving concepts
- latest duplicate audits
- compact slate context
- relevant human notes
- human direction

There is no default selection count.

### Node 25: Write Development Select

For each decision:

- set status to `DEVELOPMENT_SELECT`, `PROMISING`, or `HOLD`
- write `development_packet_json` only for selected concepts

Never overwrite a pre-existing nonblank Development Packet without explicit re-development action.

Development Select is no longer the terminal point of the normal workflow.

Every newly selected concept should proceed directly into Phase D during the same execution unless an explicit debug / stop-after-selection control is active.

## 12. Phase D: Production development

Process each newly selected concept independently.

A failure on one selected concept should not corrupt or roll back successfully packaged siblings.

### Node 26: Production Builder

Use:

- `Prompts/Generation/01-production-builder.system.md`
- `Prompts/Generation/01-production-builder.user.md`
- `Schemas/canon-bible.schema.json`

Inputs:

- `development_packet_json`
- relevant `human_notes`
- optional research packet

Output:

`canon_bible_json`

The Production Builder owns:

- final title
- final format
- genre
- logline
- core promise
- creative kernel
- world
- characters
- central relationships
- complete story
- actual ending
- signature scenes
- genre delivery
- public unresolved value
- public genre demonstrations
- series engine and season material when relevant
- continuity facts

No article is written.

No actor-casting call is required.

No score is created.

Validate against `Schemas/canon-bible.schema.json`.

Allow one structure-only repair when appropriate.

### Optional research branch

Research is not a default step.

Only run Research Grounder when:

- explicit research questions were supplied, or
- a configured high-confidence routing rule identifies factual uncertainty that materially affects canon

If research is run:

1. generate factual research packet
2. rerun Production Builder once with that packet
3. freeze the second canon

Do not create recursive research loops.

### Node 27: Render Production Treatment

Create a clean human-readable treatment deterministically from final Canon Bible.

Do not spend another model call.

Include:

- final title
- format and genre
- logline
- core promise
- creative kernel
- world
- characters
- central relationships
- complete internal story
- signature scenes
- genre delivery
- series engine / season material when relevant
- continuity facts

Keep the rendered treatment in execution memory until the social packet also validates.

## 13. Phase E: Social release packaging

### Node 28: Social Release Builder

Use:

- final Canon Bible
- optional `human_campaign_notes`
- current brand / visual / social / integrity governance
- `Prompts/Generation/10-ig-asset-packet-builder.system.md`
- `Prompts/Generation/10-ig-asset-packet-builder.user.md`
- `Schemas/ig-asset-packet.schema.json`

Output:

`ig_packet_json`

Do not pass an article.

Do not pass `ncs_score`.

The packet must be ready to paste verbatim into a separate manual asset-generation chat.

### Node 29: IG Packet QA

Validate the exact object that will be persisted.

Require:

- `version = ncs_ig_v2`
- final title matches Canon Bible
- format matches Canon Bible
- FILM -> MOVIE IDEA
- SERIES / LIMITED_SERIES -> SHOW IDEA
- exactly six slides
- correct slide types
- every slide has `footer_brand = Never Coming Soon`
- page numbers exactly `01 / 06` through `06 / 06`
- Slide 2 body copy nonblank
- Slide 3 has 2 to 4 featured characters
- each Slide 3 character has nonblank final copy and `portrait_prompt`
- Slide 4 body copy nonblank
- poster fields and image prompt nonblank
- Slide 6 slogan matches schema
- caption nonblank
- visual direction nonblank
- no em dash character in public copy
- no backstage technology language
- no false real-world participation claims

Allow one packet-only structure/integrity repair.

Do not rerun Production Builder merely because packet formatting failed.

If packet validation remains invalid, leave the row in `DEVELOPMENT_SELECT`.

## 14. Phase F: Durable delivery

### Node 30: Persist Production Treatment

Only after Canon Bible and IG Packet both validate, write:

`Never Coming Soon / Drafts / [FINAL TITLE] / [FINAL TITLE] - Production Treatment`

The legacy Sheet field `draft_url` points to this treatment.

Avoid creating orphan treatment documents for failed packet runs when practical.

### Node 31: Final Ideas update

After Drive persistence succeeds, update the same Ideas row with:

- `final_title`
- `draft_url`
- `ig_packet_json`
- `status = DRAFTED`

Do not require or generate `ncs_score`.

If a historical score already exists, leave it untouched.

Never populate `published_url`.

Do not overwrite source Ideation fields with final-production choices.

## 15. Meaning of DRAFTED

`DRAFTED` means:

- the concept survived Ideation and Development Selection
- complete internal production canon was built successfully
- a durable human-readable production treatment exists
- a schema-valid six-slide social release packet exists
- the production is ready for manual asset generation and human judgment

It does not mean:

- a long-form article exists
- an editorial score exists
- actors have been cast
- social assets have been rendered
- the production is approved for publication

## 16. Manual asset-generation boundary

n8n stops at `ig_packet_json`.

Do not build image generation, image editing, slide composition, or carousel export nodes into this workflow.

Asset generation is intentionally manual because it is high-value creative work.

The packet must therefore contain enough exact copy and visual direction that the manual asset-generation chat does not need to make editorial decisions.

## 17. Optional later long-form path

Long-form editorial is outside the normal unified workflow.

If a human later wants an article, website feature, or email edition:

1. load the frozen Production Treatment / Canon Bible
2. run one purpose-built long-form writer
3. optionally review or revise when the artifact warrants the cost
4. persist separately

Do not make long-form writing a prerequisite for social packaging.

## 18. Structured-output repair

For schema-invalid model output:

1. retry once using the original response plus exact validation error
2. instruct the same model to repair structure only
3. preserve creative content where possible

If repair fails:

- record the item-level error
- continue siblings when safe
- never write malformed JSON to durable fields

## 19. Google Sheets write rules

- use `idea_id` as update key
- preserve `created_at`
- never erase `human_notes`
- stringify JSON exactly once
- leave not-yet-produced structured cells blank
- do not regenerate creative content merely because persistence failed
- do not write partial final fields before the full selected-production package succeeds

## 20. Failure behavior

Every generated seed should remain persisted with an explicit lifecycle state whenever technically possible.

For a selected concept:

- if Production Builder fails, leave it `DEVELOPMENT_SELECT`
- if IG Packet fails validation, leave it `DEVELOPMENT_SELECT`
- if Drive persistence fails, leave it `DEVELOPMENT_SELECT`
- only write final fields and `DRAFTED` after the complete package succeeds

One selected concept's failure should not erase successfully completed siblings.

## 21. Execution summary

Return one operational summary containing:

### Ideation

- total seeds generated
- seed-stage duplicate count
- curator DEVELOP / PROMISING / HOLD counts
- expanded-stage duplicate count
- final Development Select count

### Production packaging

- selected idea IDs attempted
- Production Builder successes / failures
- research branches used
- IG Packet validation successes / failures
- treatment URLs
- rows advanced to DRAFTED

### Diagnostics

- schema repair attempts
- item-level errors
- unresolved hard blockers

Do not run another creative review merely to generate the summary.

## 22. Non-goals

Do not build by default:

- separate Generation n8n workflow
- automatic asset generation
- article drafting
- actor-casting agent
- Forensic scoring
- automatic revision loops
- vector database
- separate Productions Sheet
- durable internal-agent-output columns
- automatic publication

The unified workflow should remain legible, debuggable, and cost-conscious.
