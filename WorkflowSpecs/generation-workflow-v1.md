# Never Coming Soon
## n8n Generation Workflow Specification v1.0

## 1. Objective

Build one n8n workflow that starts from a `DEVELOPMENT_SELECT` idea and ends with the strongest automated Never Coming Soon edition ready for human review.

The workflow develops the imaginary production before it writes the public article.

Its terminal state is:

`READY_FOR_HUMAN_REVIEW`

The workflow does not publish automatically.

## 2. Systems

- GitHub: source of truth for governance, prompts, schemas, and this specification
- Ideation Google Sheet: source Development Packet and idea-level human notes
- Productions Google Sheet: persistent Generation state
- Primary LLM: strongest available OpenAI reasoning/creative model
- Independent critic model: strongest available Anthropic model when configured; otherwise a clean-context call to the primary model
- Web research: conditional and narrowly scoped
- n8n Code nodes: deterministic IDs, routing, JSON handling, QA, and state updates

## 3. Source snapshot

At the beginning of each run, resolve the current commit SHA of the GitHub `main` branch.

Store it as `source_commit_sha`.

Fetch every required runtime file using that exact commit SHA as the ref.

A single execution must never mix governance versions.

## 4. Runtime files

### Governance

Load:

- `Governance/ncs-brand-constitution.md`
- `Governance/ncs-generation-review-revision-os.md`
- `Governance/ncs-story-development-standard.md`
- `Governance/ncs-research-grounding-standard.md`
- `Governance/ncs-casting-standard.md`
- `Governance/ncs-editorial-anatomy.md`
- `Governance/ncs-voice-constitution.md`
- `Governance/ncs-editorial-quality-standard.md`
- `Governance/ncs-generation-data-contract.md`
- `Governance/ncs-catalog-memory-standard.md`

### Prompts

Load all files under:

`Prompts/Generation/`

### Schemas

Load:

- `Schemas/production-development.schema.json`
- `Schemas/research-packet.schema.json`
- `Schemas/story-challenge.schema.json`
- `Schemas/canon-bible.schema.json`
- `Schemas/casting-plan.schema.json`
- `Schemas/edition-plan.schema.json`
- `Schemas/edition-draft.schema.json`
- `Schemas/editorial-review.schema.json`
- `Schemas/revision-output.schema.json`

Fail the run if required source files cannot be loaded.

Do not silently substitute stale hard-coded prompt text.

## 5. Prompt assembly

For each model call:

1. use the full text of the agent's `.system.md`
2. append the full text of only the governance documents listed under that prompt's `AUTHORITATIVE GOVERNANCE`
3. clearly delimit each governance document
4. render the matching `.user.md` with runtime data
5. enforce the matching JSON Schema through structured output when supported

Do not inject unrelated governance.

More context is not automatically better.

## 6. Trigger

Support a manual trigger with:

- `idea_id` required
- `force_redevelopment` optional boolean, default false

The run should normally start from one selected idea.

Do not batch multiple full productions in one Generation execution in v1.

## 7. Load source idea

Read the Ideation `Ideas` sheet by `idea_id`.

Require:

- status = `DEVELOPMENT_SELECT`, unless `force_redevelopment = true`
- nonblank `development_packet_json`

Also read:

- `human_notes`
- current title, format, genre, premise, and creative kernel for traceability

Do not use Ideation scorecards or duplicate audits as downstream writer context.

## 8. Create or load Production row

Use `Governance/ncs-generation-data-contract.md`.

Default:

`NCS-I-000185` -> `NCS-P-000185`

If no Production row exists, create one and snapshot:

- production_id
- idea_id
- created_at
- status = `DEVELOPING`
- development_packet_json
- human_notes if copied only as read context, never overwrite human-owned Production notes later

If a Production row already exists and `force_redevelopment` is false, do not silently overwrite completed work.

## 9. Node sequence

### Node 1: Manual Trigger

Receive `idea_id` and optional `force_redevelopment`.

### Node 2: Resolve GitHub Source Commit

Resolve `main` HEAD and store `source_commit_sha`.

### Node 3: Load Runtime Files

Fetch all required governance, prompts, and schemas at `source_commit_sha`.

### Node 4: Read Ideation Source Row

Fetch selected idea by `idea_id`.

Validate Development Packet exists.

### Node 5: Create or Load Production Workspace

Create Production row or load existing row according to overwrite rules.

### Node 6: Production Developer

Use:

- `Prompts/Generation/01-production-developer.system.md`
- `Prompts/Generation/01-production-developer.user.md`
- `Schemas/production-development.schema.json`

Inputs:

- development packet
- idea-level human notes

Do not send broad NCS catalog history.

Write output to `development_blueprint_json`.

Set status `DEVELOPING`.

### Node 7: Research Needed?

Read `development_blueprint_json.research_requests`.

If empty, skip research and leave `research_packet_json` blank.

If nonempty, continue.

### Node 8: Research Grounder, conditional

Set status `RESEARCHING`.

Use:

- `Prompts/Generation/02-research-grounding.system.md`
- `Prompts/Generation/02-research-grounding.user.md`
- `Schemas/research-packet.schema.json`

Enable web research only for this node.

Research exactly the requested questions.

Write `research_packet_json`.

### Node 9: Build Story-Challenge Catalog Memory

Read compact historical NCS memory from Ideation and, when useful, developed Production canon.

Provide only what helps detect internal repetition:

- idea_id
- status
- premise
- creative_kernel
- memory signature
- compact production core for previously developed productions when available

Do not give the Challenger full old articles.

### Node 10: Story Challenger

Set status `CHALLENGING`.

Use:

- `Prompts/Generation/03-story-challenger.system.md`
- `Prompts/Generation/03-story-challenger.user.md`
- `Schemas/story-challenge.schema.json`

Preferred provider: strongest available independent Anthropic model.

Fallback: strongest primary OpenAI model in a fresh clean context.

Inputs:

- Development Packet
- development blueprint
- research packet if any
- compact relevant NCS catalog memory

Write `story_challenge_json`.

### Node 11: Canon Builder

Use:

- `Prompts/Generation/04-canon-builder.system.md`
- `Prompts/Generation/04-canon-builder.user.md`
- `Schemas/canon-bible.schema.json`

Inputs:

- Development Packet
- first blueprint
- research packet if any
- story challenge
- human notes

The Canon Builder decides which challenge notes to use.

Write `canon_bible_json`.

Set status `CANON_READY`.

### Node 12: Build Casting Memory

Parse prior `casting_json` values from Production history.

Create a compact summary of recent/frequent actor usage.

Casting memory creates awareness, not bans.

### Node 13: Casting Director

Use:

- `Prompts/Generation/05-casting-director.system.md`
- `Prompts/Generation/05-casting-director.user.md`
- `Schemas/casting-plan.schema.json`

Inputs:

- canon bible
- compact casting memory

Do not send Challenger notes or Ideation scores.

Write `casting_json`.

Set status `CAST_READY`.

### Node 14: Edition Architect

Use:

- `Prompts/Generation/06-edition-architect.system.md`
- `Prompts/Generation/06-edition-architect.user.md`
- `Schemas/edition-plan.schema.json`

Inputs:

- canon bible
- casting plan

Write `edition_plan_json`.

Set status `EDITION_PLANNED`.

### Node 15: Load Gold-Standard Style Examples

When approved style-example files exist in GitHub, load only the files explicitly designated as Generation writing calibration.

At initial v1, this node may return blank until the first gold-standard NCS edition is added.

Do not use random historical NCS pieces as style examples.

### Node 16: Edition Writer

Use:

- `Prompts/Generation/07-edition-writer.system.md`
- `Prompts/Generation/07-edition-writer.user.md`
- `Schemas/edition-draft.schema.json`

Clean input only:

- canon bible
- casting plan
- edition plan
- approved style examples if configured

Do not send:

- story challenge
- development blueprint alternatives
- Ideation scorecards
- duplicate audits
- broad catalog memory

Write `draft_v1_markdown` from `article_markdown`.

Set status `DRAFTED`.

### Node 17: Deterministic Pre-Review Checks

Check and record warnings for:

- blank draft
- em dash character
- obvious backstage-process phrases such as references to prompts, model usage, AI assistance, generation systems, or tooling; do not flag legitimate story content merely because the production itself concerns AI or technology
- word count far outside Editorial Anatomy guidance

Do not automatically rewrite.

### Node 18: Forensic Editor

Use:

- `Prompts/Generation/08-forensic-editor.system.md`
- `Prompts/Generation/08-forensic-editor.user.md`
- `Schemas/editorial-review.schema.json`

Preferred provider: strongest available independent Anthropic model.

Fallback: strongest primary OpenAI model in fresh context.

Inputs:

- canon bible
- edition plan
- draft

Write `editorial_review_json`.

Set status `REVIEWED`.

### Node 19: Route by revision_route

Read `editorial_review_json.revision_route`.

Allowed routes:

- NONE
- PROSE
- EDITION
- CANON

Track `rescue_cycle_count` in n8n execution data.

Maximum automated rescue cycles in v1: 1.

### Node 20A: NONE

Copy `draft_v1_markdown` to `draft_final_markdown` if final is blank.

Proceed to final QA.

### Node 20B: PROSE

Run Revision Writer once.

Use:

- `Prompts/Generation/09-revision-writer.system.md`
- `Prompts/Generation/09-revision-writer.user.md`
- `Schemas/revision-output.schema.json`

Inputs:

- latest canon
- latest edition plan
- current draft
- editorial review
- approved style examples

Write `draft_final_markdown`.

Proceed to final QA.

### Node 20C: EDITION

If rescue_cycle_count = 0:

1. increment rescue cycle
2. rerun Edition Architect with the editorial review appended as explicit revision context
3. rerun Edition Writer
4. rerun Forensic Editor once
5. route again

If a second deep route is requested after the rescue cycle, stop automated recursion and proceed with the strongest current draft plus unresolved warnings for human review.

### Node 20D: CANON

If rescue_cycle_count = 0:

1. increment rescue cycle
2. rerun Canon Builder with the editorial review as additional rescue context
3. overwrite `canon_bible_json`
4. rerun Casting Director because character/canon changes may affect casting
5. rerun Edition Architect
6. rerun Edition Writer
7. rerun Forensic Editor once
8. route again

If a second CANON or EDITION route is requested after the rescue cycle, stop automated recursion and deliver the strongest current version with unresolved warnings.

### Node 21: Final Revision Writer when needed after rescue

If the post-rescue Forensic Editor returns `PROSE`, run Revision Writer once.

If it returns `NONE`, use the latest draft as final.

### Node 22: Deterministic Final QA

Check:

- nonblank final Markdown
- no em dash character
- no obvious backstage-process references; legitimate in-story references to AI or technology are allowed
- word count is plausible for format
- article title is nonblank
- JSON state fields remain valid

Do not mutate creative prose automatically.

Store warnings in execution summary.

### Node 23: Ready for Human Review

Write final Markdown to `draft_final_markdown`.

Set status:

`READY_FOR_HUMAN_REVIEW`

Return a concise execution summary.

## 10. Model roles

Use the highest-capability practical models, not cheaper models by default.

Recommended behavior:

- Production Developer: strongest OpenAI model, high reasoning and high creativity
- Research Grounder: strong model with web search, high factual discipline
- Story Challenger: strongest independent Anthropic model when available, high reasoning and low creative variance
- Canon Builder: strongest OpenAI model, highest reasoning
- Casting Director: strongest OpenAI model, moderate reasoning and creative range
- Edition Architect: strongest OpenAI model, high reasoning
- Edition Writer: strongest OpenAI model, high writing capability with moderate creative variance
- Forensic Editor: strongest independent Anthropic model when available, high reasoning and low variance
- Revision Writer: strongest OpenAI model, high writing capability and controlled creativity

Do not introduce model diversity into creative stages merely for novelty.

The independent model is most useful in challenge and editorial review.

## 11. Context hygiene matrix

### Production Developer sees

- Development Packet
- human notes

No catalog history.

### Research Grounder sees

- blueprint context
- explicit research requests

No catalog history.

### Story Challenger sees

- Development Packet
- blueprint
- research packet
- compact relevant catalog memory

### Canon Builder sees

- Development Packet
- blueprint
- research packet
- story challenge
- human notes

### Casting Director sees

- canon
- casting-history summary

### Edition Architect sees

- canon
- casting

### Edition Writer sees

- canon
- casting
- edition plan
- approved style examples

### Forensic Editor sees

- canon
- edition plan
- draft

### Revision Writer sees

- latest canon
- latest edition plan
- current draft
- editorial review
- approved style examples

This separation is mandatory.

## 12. Structured output repair

If a model returns invalid JSON or schema-invalid output:

1. retry once using the same model
2. include the original response and exact validation error
3. instruct the model to repair structure only
4. do not invite substantive creative rewriting unless required data is genuinely missing

If repair fails, record the error and stop any downstream stage that would corrupt state.

## 13. Persistence safety

- update Production rows by `production_id`, not visible row number
- preserve `production_id`, `idea_id`, and `created_at`
- never overwrite `human_notes`
- do not regenerate creative content merely because a Sheet write failed
- retry transient writes with exponential backoff
- parse JSON once and stringify once
- store Markdown as Markdown, not JSON

## 14. Overwrite dependencies during rescue

When canon changes, invalidate and regenerate:

- casting
- edition plan
- draft
- editorial review
- final draft

When only edition architecture changes, preserve canon and casting but regenerate:

- edition plan
- draft
- editorial review
- final draft

When only prose changes, preserve all upstream creative state.

## 15. No hidden publication decision

The automated system does not create a final `PUBLISH` status.

Human review decides what happens next.

Generation should rescue before giving up, but it should not create endless loops to avoid showing imperfect work to the editor.

## 16. V1 non-goals

Do not build unless later requested:

- visual generation
- poster generation
- social carousel generation
- automated publication
- automated newsletter sending
- multi-row character database
- multi-tab Production architecture
- dedicated title agent
- dedicated ending agent
- dedicated scene agent
- endless self-revision loops

Visual production should be a separate future workflow after text canon and edition are stable.

## 17. End summary

Return:

- source_commit_sha
- production_id
- idea_id
- models actually used
- whether research ran
- initial challenge verdict
- whether canon rescue occurred
- whether edition rescue occurred
- final editorial route
- deterministic QA warnings
- final word count
- final status
- unresolved issues if any
