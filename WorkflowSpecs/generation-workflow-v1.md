# Never Coming Soon
## n8n Generation Workflow Specification v1.1

## 1. Objective

Build one n8n workflow that starts from a `DEVELOPMENT_SELECT` idea and ends with the strongest automated Never Coming Soon edition ready for human review.

The workflow develops the imaginary production before it writes the public article.

Terminal state:

`READY_FOR_HUMAN_REVIEW`

The workflow does not publish automatically.

## 2. Systems

- GitHub: source of truth for governance, prompts, schemas, approved gold examples, and this specification
- Ideation Google Sheet: source Development Packet and idea-level human notes
- Productions Google Sheet: persistent Generation state
- Primary LLM: strongest available OpenAI reasoning/creative model
- Independent critic model: strongest available Anthropic model when configured; otherwise clean-context primary-model fallback
- Web research: conditional and narrowly scoped
- n8n Code nodes: deterministic IDs, routing, diagnostics, JSON handling, QA, and state updates

## 3. Source snapshot

At the beginning of each run, resolve the current commit SHA of GitHub `main` and store it as `source_commit_sha`.

Fetch every required runtime file using that exact commit SHA. A single execution must never mix governance versions.

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

Load all files under `Prompts/Generation/`.

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

### Approved writing calibration

Load:

- `Examples/Gold/film-01-the-tell.md`

Do not automatically add every completed NCS edition to writing calibration.

Fail the run if required source files cannot be loaded. Do not silently substitute stale hard-coded prompt text.

## 5. Prompt assembly

For each model call:

1. use the full text of the agent's `.system.md`
2. append the full text of only the governance documents listed under that prompt's `AUTHORITATIVE GOVERNANCE`
3. clearly delimit each governance document
4. render the matching `.user.md` with runtime data
5. enforce the matching JSON Schema through structured output when supported

Gold examples are loaded separately and supplied only to writing stages that explicitly use them.

## 6. Trigger

Support a manual trigger with:

- `idea_id` required
- `force_redevelopment` optional boolean, default false

Do not batch multiple full productions in one Generation execution in v1.

## 7. Source and Production workspace

Read the Ideation row by `idea_id`.

Require status `DEVELOPMENT_SELECT` unless forced and require nonblank `development_packet_json`.

Read human notes and basic traceability fields. Do not use Ideation scorecards or duplicate audits as downstream writer context.

Create or load one Production row according to `Governance/ncs-generation-data-contract.md`. Preserve immutable IDs, `created_at`, and human-owned notes.

## 8. Node sequence

### Node 1: Manual Trigger

Receive `idea_id` and optional `force_redevelopment`.

### Node 2: Resolve GitHub Source Commit

Resolve `main` HEAD and store `source_commit_sha`.

### Node 3: Load Runtime Files

Fetch governance, prompts, schemas, and approved calibration files at `source_commit_sha`.

### Node 4: Read Ideation Source Row

Fetch the selected idea and validate the Development Packet.

### Node 5: Create or Load Production Workspace

Create the Production row or resume according to overwrite rules.

### Node 6: Production Developer

Inputs: Development Packet and idea-level human notes.

Write `development_blueprint_json`. Do not send broad catalog history.

### Node 7: Research Needed?

Read `development_blueprint_json.research_requests`.

### Node 8: Research Grounder, conditional

Research exactly the requested questions with web access and write `research_packet_json`.

### Node 9: Build Story-Challenge Catalog Memory

Build compact repetition memory only.

### Node 10: Story Challenger

Inputs: Development Packet, blueprint, research if any, compact catalog memory.

Preferred provider: strongest configured independent Anthropic model. Fallback: strongest primary OpenAI model in fresh context.

Write `story_challenge_json`.

### Node 11: Canon Builder

Inputs: Development Packet, blueprint, research if any, story challenge, human notes.

Write `canon_bible_json` and set `CANON_READY`.

### Node 12: Build Casting Memory

Create compact recent/frequent actor usage summary.

### Node 13: Casting Director

Inputs: canon bible and compact casting memory.

Write `casting_json` and set `CAST_READY`.

### Node 14: Edition Architect

Inputs: canon bible and casting plan.

Write `edition_plan_json` and set `EDITION_PLANNED`.

### Node 15: Load Gold-Standard Style Examples

Load explicitly approved calibration files from the runtime snapshot.

Current approved film example: `Examples/Gold/film-01-the-tell.md`.

Gold examples teach craft and judgment. They are not story templates.

### Node 16: Edition Writer

Clean inputs only: canon bible, casting plan, edition plan, approved style examples.

Write `draft_v1_markdown` and set `DRAFTED`.

### Node 17: Deterministic Pre-Review Diagnostics

Run noncreative diagnostics and pass the results to the Forensic Editor.

Record at minimum:

- blank draft warning
- approximate word count
- em dash presence
- obvious backstage-process phrase warnings
- total paragraph count
- count of non-dialogue one-sentence paragraphs
- longest consecutive run of non-dialogue one-sentence paragraphs
- paragraph indices or locations for one-sentence runs when practical

Implementation note for paragraph analysis:

- split Markdown into logical paragraphs after normalizing blank lines
- exclude headings, horizontal rules, list items, studio-card furniture, and isolated dialogue lines from the non-dialogue one-sentence metric when reliably identifiable
- sentence counting may be heuristic and should never rewrite prose
- diagnostics are warnings and context, not automatic failures

The purpose is to make habitual drumbeat prose visible before review. Dialogue, suspense, comic isolation, and deliberate camera-like sequences may legitimately use isolated lines.

Do not automatically rewrite.

### Node 18: Forensic Editor

Inputs:

- canon bible
- edition plan
- draft
- deterministic pre-review diagnostics

Use `Prompts/Generation/08-forensic-editor.system.md` and `Schemas/editorial-review.schema.json`.

Preferred provider: strongest configured independent Anthropic model. Fallback: strongest primary OpenAI model in fresh context.

Write `editorial_review_json` and set `REVIEWED`.

### Node 19: Route by `revision_route`

Allowed routes:

- NONE
- PROSE
- EDITION
- CANON

Track `rescue_cycle_count`. Maximum automated deep rescue cycles in v1: 1.

### Node 20A: NONE

Use current draft as final if final is blank.

### Node 20B: PROSE

Run Revision Writer once with latest canon, latest edition plan, current draft, editorial review, and approved style examples.

### Node 20C: EDITION

If rescue cycle is unused:

1. increment rescue cycle
2. rerun Edition Architect with review context
3. rerun Edition Writer
4. rerun deterministic pre-review diagnostics
5. rerun Forensic Editor
6. route again

If another deep route is requested afterward, stop recursion and deliver the strongest current draft with unresolved warnings.

### Node 20D: CANON

If rescue cycle is unused:

1. increment rescue cycle
2. rerun Canon Builder with review context
3. regenerate casting
4. regenerate edition plan
5. rerun Edition Writer
6. rerun deterministic pre-review diagnostics
7. rerun Forensic Editor
8. route again

If another deep route is requested afterward, stop recursion and deliver the strongest current version with unresolved warnings.

### Node 21: Final Revision Writer when needed after rescue

If the post-rescue review returns `PROSE`, run Revision Writer once. If `NONE`, use the latest draft.

### Node 22: Deterministic Final QA

Check and record:

- nonblank final Markdown
- no em dash character
- no obvious backstage-process references
- plausible word count
- nonblank title
- valid JSON state
- total paragraph count
- count of non-dialogue one-sentence paragraphs
- longest consecutive run of non-dialogue one-sentence paragraphs

Compare paragraph-rhythm diagnostics to the pre-review version when available.

If revision materially worsened the one-sentence-paragraph pattern, surface a warning for human review.

Do not mutate creative prose automatically.

### Node 23: Ready for Human Review

Write `draft_final_markdown`, set `READY_FOR_HUMAN_REVIEW`, and return a concise execution summary.

## 9. Model roles

Use the highest-capability practical models, not cheaper models by default.

Recommended behavior:

- Production Developer: strongest OpenAI model, high reasoning and creativity
- Research Grounder: strong model with web search and factual discipline
- Story Challenger: strongest independent Anthropic model when configured, otherwise clean-context strongest OpenAI
- Canon Builder: strongest OpenAI model, highest reasoning
- Casting Director: strongest OpenAI model, moderate reasoning and creative range
- Edition Architect: strongest OpenAI model, high reasoning
- Edition Writer: strongest OpenAI model, high writing capability with controlled creative variance
- Forensic Editor: strongest independent Anthropic model when configured, otherwise clean-context strongest OpenAI
- Revision Writer: strongest OpenAI model, high writing capability and controlled creativity

## 10. Context hygiene

Production Developer sees Development Packet + human notes.

Research Grounder sees blueprint context + explicit research requests.

Story Challenger sees Development Packet + blueprint + research + compact catalog memory.

Canon Builder sees Development Packet + blueprint + research + challenge + human notes.

Casting Director sees canon + casting-history summary.

Edition Architect sees canon + casting.

Edition Writer sees canon + casting + edition plan + approved gold examples.

Forensic Editor sees canon + edition plan + draft + deterministic diagnostics.

Revision Writer sees latest canon + latest edition plan + current draft + review + approved gold examples.

This separation is mandatory.

## 11. Structured output repair

If model output is invalid JSON or schema-invalid, retry once with the same model using the original response and exact validation error, instructing structure-only repair unless required data is genuinely missing.

If repair fails, stop downstream stages that would corrupt state.

## 12. Persistence safety

- update Production rows by `production_id`, not visible row number
- preserve immutable IDs and `created_at`
- never overwrite `human_notes`
- do not regenerate creative content merely because a Sheet write failed
- retry transient writes with backoff
- parse JSON once and stringify once
- store Markdown as Markdown

## 13. Overwrite dependencies during rescue

When canon changes, regenerate casting, edition plan, draft, review, and final draft.

When edition architecture changes, preserve canon and casting but regenerate edition plan, draft, review, and final draft.

When only prose changes, preserve all upstream creative state.

## 14. No hidden publication decision

The automated system does not create a final `PUBLISH` status. Human review decides what happens next.

## 15. V1 non-goals

Do not build unless later requested: visual generation, poster generation, social carousel generation, automated publication, automated newsletter sending, multi-row character database, multi-tab Production architecture, dedicated title/ending/scene agents, or endless self-revision loops.

## 16. End summary

Return source commit SHA, production ID, idea ID, models actually used, whether research ran, challenge verdict, rescue information, final editorial route, deterministic QA warnings, paragraph-rhythm diagnostics, final word count, final status, and unresolved issues if any.
