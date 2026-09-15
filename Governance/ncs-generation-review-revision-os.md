# Never Coming Soon
## Generation, Review and Revision Operating System v2.6

## 1. Purpose

Generation begins after an idea becomes `DEVELOPMENT_SELECT`.

Its job is to create a strong imaginary movie or show and turn it into a human-reviewable Never Coming Soon article efficiently.

**Draft Generation is not Publish Prep.**

The normal run should answer:

**Did we make a movie or show, and an article about it, that a human editor wants to keep working on?**

The workflow should preserve creative quality while avoiding paid model calls that do not materially improve the first draft.

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

## 4. Persistence architecture

Use:

- Ideas row for durable lifecycle state
- n8n execution memory for intermediate creative artifacts
- Google Drive for the delivered public draft
- GitHub for source-governed prompts, schemas, standards, and approved calibration

Use `idea_id` as the only durable identifier.

Do not create a separate Productions state table.

## 5. Draft Generation stages

The normal Draft Generation path is:

1. source selection and eligibility
2. save `pre_generation_status` and set `GENERATING`
3. Production Developer
4. optional narrow research only when requested
5. Canon Builder with internal independent challenge
6. canon freeze
7. Casting Director
8. Edition Architect
9. Gold/style calibration load for Writer only
10. Edition Writer
11. deterministic pre-review diagnostics
12. Forensic Editor
13. deterministic final article QA
14. Google Drive delivery
15. final Ideas row update to `DRAFTED`

Do not run a standalone Story Challenger in the normal path.

Do not automatically run Revision Writer, EDITION rescue, CANON rescue, a duplicate final Forensic review, IG Asset Packet Builder, or IG Packet QA before the human sees the draft.

## 6. Why the Challenger is folded into Canon

The Canon Builder is the second major creative pass and the final development authority.

Before freezing canon it must independently stress-test the Production Developer's work for:

- obvious first interpretation
- weak causality
- rigged moral conflict
- generic or functional characters
- weak genre delivery
- over-designedness
- format mismatch
- season predictability
- missed stronger versions

It should solve material problems it agrees are real without outputting a separate challenge memo.

This preserves a second creative perspective while removing one paid stage.

## 7. Production development before prose

Production Developer remains a premium creative call.

Development should establish character agency, relationships, causality, pressure, genre delivery, earned ending, ordinary world texture, and enough scene fertility to prove the production works.

Do not cut this stage merely to save tokens.

## 8. Selective research

Research is conditional.

Run it only when Production Developer returns genuinely nonempty `research_requests` and real-world grounding would materially improve the production.

Do not research a setting merely because research is possible.

## 9. Canon Freeze

After Canon Builder succeeds, downstream agents should represent the frozen production rather than casually reinvent it.

For film, canon should know the complete story and ending, central relationships, major scenes, genre delivery, world texture, and protected public value.

For series, canon should also know the recurring engine, Season One movement, concrete episode possibilities, actual finale, and future engine when relevant.

For limited series, canon should know the contained ending and chapter logic.

Automatic CANON rescue is not part of normal Draft Generation. Foundational redevelopment is an explicit later action.

## 10. Casting

Casting remains separate in Phase 1.

Character first. Actor second. Fit before fame.

Provide compact casting memory. Do not burden Casting with Gold articles or unrelated editorial governance.

Casting may use a capable cheaper creative model than the major development, writing, or forensic stages when practical.

## 11. Edition architecture

Keep Edition Architect.

It decides opening strategy, public character focus, section flow, compression, signature scenes, scene ownership, spoiler strategy, genre demonstration, ordinary world detail, spectatorship, motif restraint, and Finish convergence.

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

## 14. Forensic Editor

Keep one fresh cold-read Forensic Editor call.

Review:

1. production quality
2. genre execution
3. edition strategy and format execution
4. public integrity
5. prose
6. surface issues

Return an honest holistic score for the exact draft plus material notes and a recommended route:

- `NONE`
- `PROSE`
- `EDITION`
- `CANON`

During Draft Generation, that route is **advisory**.

Do not automatically execute it before human review.

The human editor may later choose whether the piece deserves no work, a light polish, edition restructuring, or redevelopment.

## 15. Draft scoring

The single Forensic review normally scores the exact article delivered because normal Draft Generation does not revise prose after review.

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

## 17. DRAFTED contract

When the article passes technical checks, write the Google Doc and then update the Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `status = DRAFTED`

`DRAFTED` means the article exists and is ready for human review.

It does not require an IG packet.

It does not mean publication-ready, score >= 8, Gold quality, or no remaining notes.

## 18. Social assets are Publish Prep

Do not run IG Asset Packet Builder or IG Packet QA during normal Draft Generation.

Social asset preparation happens only after explicit human interest in publishing a production.

Existing `ig_packet_json` values on older `DRAFTED` rows remain valid historical outputs and should not be cleared by Draft Generation.

## 19. Revision belongs after human selection

Revision Writer remains available for later Publish Prep or explicit redevelopment.

Human-selected Publish Prep may run:

- targeted PROSE revision
- optional final Forensic review when changed prose needs a fresh score
- IG Asset Packet Builder
- IG QA
- future image/social handoff

EDITION or CANON redevelopment should require explicit human action.

Do not spend premium model calls polishing every generated idea before the human has decided it deserves publication effort.

## 20. Context-cost discipline

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

Do not send social/visual governance to article-generation agents.

Preserve stable prompt prefixes where practical so provider caching can work.

## 21. Failure and force-redevelopment recovery

Keep the `GENERATING` lock.

Before setting it, store `pre_generation_status`.

If a run fails before draft delivery, restore the exact row to `pre_generation_status` only when its current status is still `GENERATING`.

For a normal new run, restore `DEVELOPMENT_SELECT`.

For failed force redevelopment, restore `DRAFTED` and preserve the prior successful final fields and Drive artifact.

The Error Recovery companion should recover the exact `idea_id`; do not reset arbitrary rows.

## 22. Human review and publication

Generation ends at `DRAFTED`.

The human editor decides whether the production is excellent, good, needs a light edit, needs redevelopment, or should never publish.

A later publishing workflow may move:

`DRAFTED` -> `PUBLISHED`

The system should optimize for a strong first draft, not autonomous perfection.
