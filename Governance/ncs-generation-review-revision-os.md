# Never Coming Soon
## Generation, Review and Revision Operating System v2.8

## 1. Purpose

Generation begins after an idea becomes `DEVELOPMENT_SELECT`.

Its job is to create a strong imaginary movie or show, turn it into a human-reviewable Never Coming Soon article efficiently, and produce the canonical IG/social handoff needed by downstream media asset generation.

The workflow should preserve creative quality while avoiding paid model calls that do not materially improve the generated package.

## 2. Input

Required source:

- one Ideas row
- `development_packet_json`
- source `idea_id`
- nonblank `human_notes`, if any

The Development Packet is a creative brief, not a locked outline.

## 3. Creative authority

Generation may change title, format, genre emphasis, protagonist, supporting characters, relationships, setting, era, world logic, plot mechanics, major turns, climax, ending, episode structure, signature scenes, and casting.

The governing obligation is:

**Preserve or improve the creative kernel. Treat every other element as provisional.**

A strong idea should remain easy to feel after development. Do not bury the kernel under machinery merely because Generation can invent more.

## 4. Persistence architecture

Use:

- Ideas row for durable lifecycle state
- n8n execution memory for intermediate creative artifacts
- Google Drive for the delivered public draft
- GitHub for source-governed prompts, schemas, standards, and approved calibration

Use `idea_id` as the only durable identifier.

Do not create a separate Productions state table.

Generation does not use an intermediate `GENERATING` lifecycle state.

## 5. Normal Generation stages

The normal Generation path is:

1. source selection and eligibility
2. Production Developer
3. optional narrow research only when requested
4. Canon Builder with internal independent challenge
5. canon freeze
6. Casting Director
7. Edition Architect
8. Gold/style calibration load for Writer only
9. Edition Writer
10. deterministic pre-review diagnostics
11. Forensic Editor
12. deterministic final article QA
13. IG Asset Packet Builder
14. deterministic IG Packet QA, with at most one packet-only repair if needed
15. Google Drive delivery
16. final Ideas row update to `DRAFTED`

Do not run a standalone Story Challenger in the normal path.

Do not automatically run Revision Writer, EDITION rescue, CANON rescue, or a duplicate final Forensic review before the human sees the draft.

## 6. Why the Challenger is folded into Canon

The Canon Builder is the second major creative pass and the final development authority.

Before freezing canon it must independently stress-test the Production Developer's work for:

- obvious first interpretation
- weak causality
- capable opposition that resets rather than adapts
- rigged moral conflict
- a premise that promises a double-edged mechanism but only dramatizes one edge
- generic or functional characters
- highly competent protagonists whose expertise answers every important problem
- weak subject agency when people are being protected, served, treated, coached, represented, or managed
- weak genre delivery
- over-designedness
- incoherent threat behavior or redundant antagonistic roles
- format mismatch
- season predictability
- missed stronger versions

It should solve material problems it agrees are real without outputting a separate challenge memo.

This preserves a second creative perspective while removing one paid stage.

## 7. Production development before prose

Production Developer remains a premium creative call.

Development should establish character agency, relationships, causality, pressure, genre delivery, earned ending, ordinary world texture, and enough scene fertility to prove the production works.

For mechanism-heavy or procedural stories, development should make sure competence is not the whole dramatic answer, the premise's claimed tradeoff is actually dramatized, and capable opposition changes in response to what protagonists do when appropriate.

Do not cut this stage merely to save tokens.

## 8. Selective research

Research is conditional.

Run it only when Production Developer returns genuinely nonempty `research_requests` and real-world grounding would materially improve the production.

Research is especially useful when a signature scene depends on specialized physical mechanics, safety systems, emergency egress, lock behavior, medicine, law, equipment, or another factual system whose failure would undermine the genre promise.

Do not research a setting merely because research is possible.

## 9. Canon Freeze

After Canon Builder succeeds, downstream agents should represent the frozen production rather than casually reinvent it.

For film, canon should know the complete story and ending, central relationships, major scenes, genre delivery, world texture, and protected public value.

For conflict-driven material, canon should also know enough internal threat logic to understand what antagonistic pressure wants, knows, can do, and why tactics change, even when public motive remains protected.

For series, canon should also know the recurring engine, Season One movement, concrete episode possibilities, actual finale, and future engine when relevant.

For limited series, canon should know the contained ending and chapter logic.

Automatic CANON rescue is not part of normal Generation. Foundational redevelopment is an explicit later action.

## 10. Casting

Casting remains separate in Phase 1.

Character first. Actor second. Fit before fame.

Provide compact casting memory. Do not burden Casting with Gold articles or unrelated editorial governance.

Casting may use a capable cheaper creative model than the major development, writing, or forensic stages when practical.

## 11. Edition architecture

Keep Edition Architect.

It decides opening strategy, public character focus, section flow, compression, signature scenes, scene ownership, spoiler strategy, genre demonstration, ordinary world detail, spectatorship, motif restraint, and Finish convergence.

Scene ownership applies across all public sections. THE CHARACTERS and THE DREAM CAST should not pre-spend a signature scene that belongs later, and one memorable tactic or antagonist beat should not become the primary evidence in several sections.

For film, THE MOVIE and THE SCENES should not fully stage the same event.

For television, THE SEASON should show concrete macro movement while THE EPISODES shows selected specific stories.

Architecture is valuable because it protects the Writer from turning canon into a synopsis or database dump.

## 12. Clean writer context

Edition Writer should receive only:

- canon
- casting
- edition plan
- relevant writing governance
- approved Gold calibration

Do not pass Ideation scores, duplicate audits, discarded development alternatives, broad catalog history, Story Challenger material, visual governance, or social-asset governance.

Writer remains a premium creative stage. Do not downgrade it merely to save cost.

## 13. Deterministic diagnostics

Keep cheap deterministic diagnostics before Forensic Review.

Inspect at minimum:

- blank draft warning
- approximate word count
- em dash presence
- backstage technology phrase warnings
- hard public-integrity leak matches
- required headings
- paragraph/sentence rhythm signals
- section overlap
- Cast completeness when practical

These are evidence for the editor, not automatic creative rewrite rules.

Semantic issues such as coy non-spoiler abstraction, signature-tactic saturation, archetype-card character prose, generic review scaffolding, incoherent threat behavior, or meta-cut language remain primarily Forensic Editor responsibilities unless a cheap deterministic check is reliable.

## 14. Forensic Editor

Keep one fresh cold-read Forensic Editor call.

Review:

1. production quality
2. genre execution
3. edition strategy and format execution
4. public integrity
5. prose
6. surface issues

The editor should distinguish a prose symptom from a canon problem. Repeated procedural tests, non-adaptive opposition, a protagonist whose competence answers everything, or a premise whose advertised contradiction is never dramatized may be production issues even when the article itself is well written.

Return an honest holistic score for the exact draft plus material notes and a recommended route:

- `NONE`
- `PROSE`
- `EDITION`
- `CANON`

During normal Generation, that route is advisory.

Do not automatically execute it before human review.

The human editor may later choose whether the piece deserves no work, a light polish, edition restructuring, or redevelopment.

## 15. Draft scoring

The single Forensic review normally scores the exact article delivered because normal Generation does not revise prose after review.

Do not make a second Forensic call simply to certify the same text again.

Require:

- numeric `overall_score`
- 1.0 to 10.0

Do not require `overall_score >= 8.0`.

Do not require `revision_route = NONE`.

If an exceptional repair changes substantive public prose after the review, the prior score becomes stale and the changed text must be reviewed before delivery.

## 16. Deterministic Final Article QA

Final QA separates blockers from warnings.

Block delivery for:

- blank or malformed article
- blank final title
- missing valid numeric score
- invalid required structured output
- impossible or missing required section structure
- unrepaired hard backstage/internal-process corruption that makes the article unusable

Do not block merely for:

- score below 8.0
- non-NONE recommended revision route
- mild section-overlap heuristic
- minor motif density
- minor rhythm or prose concerns
- small casting taste notes

## 17. IG Asset Packet Builder

Keep the IG Asset Packet Builder in normal Generation because `ig_packet_json` is the canonical handoff to the downstream media asset generator.

Run it only after article text, canon, final title, resolved format, and Forensic score are stable.

Inputs:

- final canon
- final title
- exact final article
- final holistic score
- resolved canonical format
- current visual/social governance

The packet must match `Schemas/ig-asset-packet.schema.json` and current social governance.

Campaign coherence should come from art direction, not literal repetition of one object on every slide.

## 18. IG Packet QA

Validate the exact final object that will be persisted after any normalization, repair, mapping, or transformation.

Require the current schema contract, including:

- `version = ncs_ig_v1`
- exact canonical format enum
- final title match
- locked three-slide types
- required NCS close copy
- nonblank caption
- non-generic Slide 2 header
- nonblank Hollywood line distinct from the poster tagline
- no em dash character in public packet copy
- no hard backstage/internal terminology
- no fake production-process credits or participation claims

If packet QA fails, allow one packet-only repair and validate again.

Do not reopen or rewrite the article because the social packet alone failed.

A structurally invalid final IG packet blocks `DRAFTED` because the media-asset handoff would be incomplete.

## 19. DRAFTED contract

When article QA and IG Packet QA pass, write the Google Doc and then update the Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` means the generated package exists and is ready for human review and downstream media asset generation.

It does not mean publication-ready, score >= 8, Gold quality, or no remaining notes.

## 20. No intermediate lifecycle state

Do not write `GENERATING` at run start.

For a new run, the row remains `DEVELOPMENT_SELECT` until successful completion.

For forced redevelopment, the row remains `DRAFTED` with prior successful final fields intact until the replacement package fully succeeds.

If a run fails, no lifecycle recovery write is required because no intermediate lifecycle mutation occurred.

Do not write partial final fields during execution.

If duplicate-run protection is needed, implement it at the orchestration/execution level rather than as another durable Sheet lifecycle state.

## 21. Revision belongs after human selection

Revision Writer remains available for later explicit revision or redevelopment.

Human-selected follow-up may run:

- targeted PROSE revision
- optional final Forensic review when changed prose needs a fresh score
- explicit EDITION redevelopment
- explicit CANON redevelopment
- regeneration of `ig_packet_json` if title, article, canon, or campaign direction materially changes

Do not spend premium model calls polishing every generated idea before the human has decided it deserves more work.

## 22. Context-cost discipline

Do not load one giant runtime bundle into every model call.

Each stage receives only relevant governance, prompt, schema, and upstream outputs.

Recommended boundaries:

**Production Developer**
- brand
- generation/development doctrine
- story development
- relationship standard when relevant
- research grounding when relevant

**Canon Builder**
- brand
- generation/development doctrine
- story development
- relationship standard when relevant
- research grounding
- Development Packet, development output, research if any, human notes

**Casting**
- canon
- casting standard
- compact cast memory

**Architect**
- canon
- casting
- editorial anatomy
- TV standard when relevant
- relationship standard when relevant
- publication integrity
- core quality guidance

**Writer**
- canon
- casting
- edition plan
- voice constitution
- editorial anatomy
- format-specific standard
- publication integrity
- Gold examples

**Forensic Editor**
- canon
- edition plan
- exact draft
- diagnostics
- quality, scoring, and public-integrity standards

**IG Asset Packet Builder**
- final canon
- final article
- final title / score / format
- visual constitution
- social asset standard
- IG prompt and schema

Do not send visual/social governance to article-generation agents.

Preserve stable prompt prefixes where practical so provider caching can work.

## 23. Human review and publication

Generation ends at `DRAFTED`.

The human editor decides whether the production is excellent, good, needs a light edit, needs redevelopment, or should never publish.

A later publishing workflow may move:

`DRAFTED` -> `PUBLISHED`

The system should optimize for a strong first generated package, not autonomous perfection.