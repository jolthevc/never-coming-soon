# Never Coming Soon
## Generation, Review and Revision Operating System v2.5

## 1. Purpose

Generation begins after an idea becomes `DEVELOPMENT_SELECT`.

Its job is to create a strong imaginary movie or show, understand it more deeply than the reader ever will, and turn it into an enjoyable Never Coming Soon edition and social handoff.

**Generation is a production-development workflow that eventually writes an article.**

It is not an autonomous perfection machine.

The desired output is a developed, coherent, pleasurable idea that makes someone think:

**I would actually watch this.**

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

Do not change material merely to demonstrate creativity.

## 4. Persistence architecture

Generation does not use a separate Productions table.

Use:

- Ideas row for durable lifecycle state
- n8n execution memory for intermediate creative artifacts
- Google Drive for final delivered article
- GitHub for source-governed prompts, schemas, standards, and Gold examples

Use `idea_id` as the only durable identifier.

## 5. Workflow stages

1. Source row selection and eligibility
2. Save pre-generation status and set `GENERATING`
3. Production Development
4. Optional Grounding Research
5. Independent Story Challenge
6. Canon Building
7. Canon Freeze
8. Casting
9. Edition Architecture with scene ownership
10. Style Calibration Load
11. Edition Drafting
12. Deterministic Pre-Review Diagnostics
13. Forensic Editorial Review
14. Targeted Revision or Controlled Rescue when materially useful
15. Final Forensic Review of the exact final draft
16. Deterministic Final Article QA
17. Social Asset Packet Builder
18. Social Packet QA on the exact final packet
19. Google Drive Delivery
20. Final Ideas Row Update to `DRAFTED`

Do not collapse these into one giant prompt.

## 6. Editorial sufficiency principle

The system should improve real problems, then stop.

Do not chase a score threshold.

Do not reopen canon because a 7.6 could theoretically become an 8.0.

Do not rewrite strong scenes, jokes, relationships, or voice merely because the critic can imagine alternatives.

Automated revision is justified when a material defect is likely to become meaningfully better through another pass.

Examples include:

- broken causality
- genre promise not delivered
- duplicated major scene treatment
- public NCS process language
- a relationship story with no demonstrated relationship
- a section that fundamentally does the wrong job
- a serious contradiction or continuity error

Minor taste notes, small elegance improvements, and nonfatal imperfections should normally remain as human-review territory.

A draft can be good enough to deliver without being Gold quality.

## 7. Production development before prose

The Production Developer creates the first serious version of the movie or show.

Development should establish causality, character agency, relationships, escalating pressure, genre delivery, an earned ending, ordinary world texture, and enough specific scenes to prove the production works.

Ask whether the production naturally supports any higher-ceiling image, set piece, reversal, formal idea, comic construction, or collision that makes somebody suddenly need the rest.

This is not a twist requirement.

## 8. Relationship stories

When romance, romantic comedy, second-chance love, or another central two-person relationship materially drives the production, apply `Governance/ncs-relationship-story-standard.md`.

The pair must work as people, not merely as endpoints of a premise mechanism.

For second-chance romance, canon must know why the first relationship ended and what would need to change before reunion could work.

For two-handers, both leads should have credible lives and stakes outside the relationship when those stakes matter to the final choice.

The public article should prove chemistry through actual interaction when romance is part of the genre promise.

## 9. Genre proof and Contained Proof candidates

Development should create enough actual genre pleasure that the public edition can prove the product without spending its decisive value.

A useful internal instrument is the **Contained Proof**: a lower-stakes sequence that demonstrates the production's primary engine or genre pleasure without spending major unresolved value.

Contained Proofs are optional.

The phrase `Contained Proof` is internal terminology and must never appear as a public article label.

## 10. Selective research

Research is conditional. Use it when real-world accuracy materially improves the production.

Research should reveal dramatic texture and constraints rather than accumulate trivia.

## 11. Independent challenge

The Story Challenger should identify dead story sections, generic character functions, weak causality, false complexity, derivative patterns, unearned endings, wrong format, missed opportunities, weak genre delivery, over-designedness, and obvious unused higher-ceiling invention.

The Challenger diagnoses. It does not own the replacement story and should not manufacture work merely to justify its existence.

## 12. Canon Builder and Canon Freeze

After the Canon Builder, the production should meaningfully exist.

For film, canon should know the complete story and ending, central relationships, major scenes, genre delivery, world texture, and protected public value.

For series, canon should also know the recurring engine, Season One movement, concrete episode possibilities, actual finale, and future engine when relevant.

For limited series, canon should know the contained ending and chapter logic.

Once frozen, downstream agents represent the production rather than casually reinventing it.

## 13. Casting after canon

Dream casting happens after characters exist.

Character first. Actor second. Fit before fame.

Provide the Casting Director a compact recent-cast memory when practical. At minimum, current Gold-example lead casts should be visible so the system does not immediately reuse the same lead performer by habit.

Public casting later should contain only roles with an actual selected performer. A paragraph in THE CAST without a named actor is a presentation error, not atmosphere.

## 14. Edition architecture and scene ownership

The Edition Architect decides how the reader should discover the production.

It chooses opening strategy, public character focus, section flow, compression, slow-down points, signature scenes, genre demonstrations, ordinary world detail, spectatorship, motif restraint, and Finish convergence.

The Edition Plan assigns substantial public sequences one primary home through `scene_ownership_plan`.

For film:

- THE MOVIE demonstrates the production's engine across movement
- THE SCENES supplies separate extractable moments
- a scene may be teased in one section and fully staged in another
- the same scene should not receive full action, dialogue, outcome, and best detail twice

For television:

- THE SEASON works at the macro level of changing relationships, pressures, fortunes, and shape
- THE EPISODES works at the specific level of memorable individual stories
- if each Season paragraph maps neatly to the episode list in order, the article is probably recapping twice
- a longer season usually benefits from spotlighting selected episodes rather than cataloging every installment
- when THE FINISH owns the finale pressure, THE EPISODES should not fully stage that finale first

## 15. Public selection doctrine

Protect whatever carries the production's major unresolved value. Give away enough genre pleasure to prove the production delivers.

Do not protect the production so aggressively that the article becomes vague.

## 16. Interpretation and narrator discipline

**Do not state meaning, mechanism, or consequence that the material is capable of delivering on its own.**

The narrator may behave like a spectator. It should not behave like the production's marketer, screenwriter, development executive, or studio.

## 17. Publication integrity

The final public article must not expose internal NCS governance or workflow vocabulary.

Follow `Governance/ncs-publication-integrity-standard.md`.

Public prose should not visibly perform the checklist.

Recurring motifs should not be underlined across every section merely because they are memorable.

## 18. Clean writer context

The Edition Writer should normally see canon, casting, edition plan, relevant governance, and approved Gold examples.

It should not receive Ideation scores, duplicate audits, discarded concepts, Challenger notes, old alternatives, or broad catalog history.

## 19. Gold-standard calibration

Current approved film calibration:

- `Examples/Gold/film-01-the-tell.md`
- `Examples/Gold/film-02-clearance.md`

Gold teaches craft and judgment. It is not a story template or a minimum score requirement.

Television still needs a formally locked Gold example.

## 20. Deterministic Pre-Review Diagnostics

Before Forensic Review, record at minimum:

- blank draft warning
- approximate word count
- em dash presence
- backstage technology phrase warnings
- hard public-integrity leak matches
- required-heading presence
- paragraph-rhythm metrics
- section-overlap diagnostic

For FILM, compare THE MOVIE and THE SCENES.

For SERIES and LIMITED_SERIES, compare THE SEASON and THE EPISODES.

Overlap detection is heuristic evidence for the editor, not an automatic rewrite rule.

## 21. Forensic editorial review

The Forensic Editor reviews:

1. production quality
2. genre execution
3. edition strategy and format execution
4. public integrity
5. prose
6. surface issues

The review returns one honest holistic score for the exact draft plus revision requirements and one route.

The score does not control lifecycle state.

The editor should choose `NONE` whenever another automated pass is unlikely to produce a material improvement, even if the honest score is in the 7s.

## 22. Revision routes

Allowed routes:

- `NONE`
- `PROSE`
- `EDITION`
- `CANON`

Use the shallowest route that can solve the actual material problem.

Typical routing:

- duplicated substantial scene treatment: EDITION
- public-integrity leakage: PROSE unless architecture caused it
- under-demonstrated genre pleasure when canon contains better material: EDITION
- foundational missing relationship logic or broken causality: CANON

Automated deep rescue remains limited to one reentry cycle.

Do not create loops whose purpose is to raise the score.

## 23. Revision philosophy

Preserve what works.

Fix the smallest set of issues that materially improves the draft.

Do not reflexively rewrite every sentence listed in a review.

When review flags duplicate scene treatment, choose one primary home and compress or replace the duplicate.

When review flags internal terminology, rewrite from the audience side.

When review flags motif overuse, keep the strongest recurrences and remove redundant underlining.

When review flags dialogue as overly written, make it more speakable rather than less specific.

## 24. Final Forensic Review

The NCS score delivered to the Sheet and Google Doc must belong to the exact article delivered.

If any revision changes article text, run the Forensic Editor again.

Require a valid numeric `overall_score` from 1.0 to 10.0.

Do not require `overall_score >= 8.0`.

Do not require final `revision_route = NONE` for delivery after the allowed automated revision path has completed.

If the final review still sees nonfatal weaknesses, deliver the best complete draft and surface them in the execution summary for human judgment.

## 25. Deterministic Final Article QA

Final QA should distinguish technical blockers from editorial warnings.

Block delivery for failures such as:

- blank or malformed final article
- blank final title
- missing valid numeric final score after repair attempt
- invalid required structured output
- impossible or missing required section structure
- unrepaired hard backstage or internal-process corruption that makes the public artifact unusable

Treat as warnings rather than lifecycle blockers when the artifact remains coherent and human-reviewable:

- score below 8.0
- non-NONE final revision route after the allowed automated path
- section-overlap heuristic still mildly positive
- minor motif density
- minor prose roughness

The goal is to avoid false-success technical state without confusing `DRAFTED` with editorial perfection.

## 26. Social Asset Packet Builder

Once final article text, canon, and score are stable, create `ig_packet_json` using current social and visual governance, prompt, and schema.

The packet always contains exactly three slide objects plus one short social caption.

The structured format value must exactly match `FILM`, `SERIES`, or `LIMITED_SERIES`.

The campaign should feel coherent without stamping the same signature motif onto all three slides. Slide 3 may carry the campaign through palette, typography, or texture alone.

## 27. Social Packet QA

Validate the exact final object that will be persisted after all repairs, normalization, mapping, or transformation.

Schema-invalid final packets remain technical blockers.

Copy-level imperfections may be repaired once without reopening the article.

## 28. Drive delivery and final Ideas state

When a complete article package passes technical completion checks, write the final Google Doc.

The document begins:

`NCS SCORE: X.X / 10`

Then a blank line, then the final article only.

After successful Drive persistence, update the same Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` means a generated artifact exists and is ready for human review. It is not a quality award and does not mean publishable without judgment.

Generation never sets `PUBLISHED` and never populates `published_url`.

## 29. Failure and force-redevelopment recovery

Before setting `GENERATING`, store `pre_generation_status`.

If the run fails before a complete artifact is delivered, restore the exact row to `pre_generation_status` only if it is still `GENERATING`.

For a normal new run, that means DEVELOPMENT_SELECT.

For a failed force redevelopment of an existing successful draft, that means DRAFTED while preserving the prior successful final fields and Drive artifact.

The error workflow must recover the exact `idea_id`.

## 30. Human review and publication

Generation ends at `DRAFTED` when the artifact exists.

The human editor decides whether the idea is excellent, merely good, needs a light edit, needs redevelopment, or should never publish.

A later human or publishing workflow may move:

`DRAFTED` -> `PUBLISHED`
