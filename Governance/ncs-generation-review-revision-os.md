# Never Coming Soon
## Generation, Review and Revision Operating System v2.4

## 1. Purpose

Generation begins after an idea becomes `DEVELOPMENT_SELECT`.

Its job is not to write an article immediately. Its job is to create the strongest possible imaginary movie or show, understand it more deeply than the reader ever will, and only then turn that production into a Never Coming Soon edition and social handoff.

**Generation is a production-development workflow that eventually writes an article.**

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
14. Targeted Revision or Controlled Rescue
15. Final Forensic Review of the exact final draft
16. Deterministic Final Article QA
17. Social Asset Packet Builder
18. Social Packet QA on the exact final packet
19. Google Drive Delivery
20. Final Ideas Row Update to `DRAFTED`

Do not collapse these into one giant prompt.

## 6. Production development before prose

The Production Developer creates the first serious version of the movie or show.

Development should establish causality, character agency, relationships, escalating pressure, genre delivery, an earned ending, ordinary world texture, and enough specific scenes to expose whether the production actually works.

Development should also ask whether the production naturally supports any higher-ceiling image, set piece, reversal, formal idea, comic construction, or collision that could make a viewer suddenly need the rest.

This is not a twist requirement.

## 7. Relationship stories

When romance, romantic comedy, second-chance love, or another central two-person relationship materially drives the production, apply `Governance/ncs-relationship-story-standard.md` during Development, Challenge, Canon, Architecture, Writing, Review, and Revision.

The central pair must work as people, not merely as endpoints of a premise mechanism.

For second-chance romance, canon must know why the first relationship ended and what would need to change before reunion could work.

For two-handers, both leads need credible lives and stakes outside the relationship.

The public article should prove chemistry through actual interaction when romance is part of the genre promise.

## 8. Genre proof and Contained Proof candidates

Development should create enough actual genre pleasure that the public edition can prove the product without spending its decisive value.

A useful internal instrument is the **Contained Proof**: a lower-stakes sequence that fully demonstrates the production's primary engine or genre pleasure without spending major unresolved value.

It demonstrates capability, not stakes, and should usually end smaller than it began.

Contained Proofs are optional.

The phrase `Contained Proof` is internal terminology and must never appear as a public article label.

## 9. Selective research

Research is conditional. Use it when real-world accuracy materially improves the production.

Research should reveal dramatic texture and constraints rather than accumulate trivia.

## 10. Independent challenge

The Story Challenger should identify dead story sections, generic character functions, weak causality, false complexity, derivative patterns, unearned endings, wrong format, missed opportunities, weak genre delivery, over-designedness, and obvious unused higher-ceiling invention.

When relationship stories are relevant, it should also challenge chemistry, breakup logic, bilateral stakes, and treatment of new partners.

The Challenger diagnoses. It does not own the replacement story.

## 11. Canon Builder and Canon Freeze

After the Canon Builder, the production should meaningfully exist.

For film, canon should know the characters, relationships, world, complete story trajectory, actual ending, emotional resolution, signature scenes, tone, genre delivery, ordinary world texture, major unresolved public value, genre pleasures available for public demonstration, and possible Contained Proof candidates.

For series, canon should also know the recurring engine, Season One spine, major character movement, concrete episode purposes, actual Season One finale, and future engine when relevant.

For limited series, canon should know the contained ending and chapter logic.

Once frozen, downstream agents represent the production rather than casually reinventing it.

## 12. Casting after canon

Dream casting happens after characters exist.

Internal casting logic may reason in terms of what the role needs. Public copy later should sound as though the performance has already been watched.

## 13. Edition architecture and scene ownership

The Edition Architect decides how the reader should discover the production.

It chooses opening strategy, public character focus, section flow, compression, slow-down points, signature scenes, unresolved value, genre demonstrations, ordinary world detail, spectatorship, genre-specific rhythm, recurring-motif budget, and Finish convergence.

The Edition Plan must also assign each substantial public sequence one primary home through `scene_ownership_plan`.

For film:

- THE MOVIE demonstrates the production's engine across movement
- THE SCENES supplies separate extractable moments
- a scene may be teased in one section and fully staged in another
- the same scene should not receive full action, dialogue, outcome, and best detail twice

For television, apply the same principle to THE SEASON and THE EPISODES.

Internal planning concepts must be translated into audience-side prose downstream.

## 14. Public selection doctrine

Protect whatever carries the production's major unresolved value. Give away enough genre pleasure to prove the production delivers.

When those overlap, a lower-stakes demonstration may prove capability without spending the stake.

Do not protect the production so aggressively that the article becomes vague.

## 15. Interpretation and narrator discipline

**Do not state meaning, mechanism, or consequence that the material is capable of delivering on its own.**

The narrator may behave like a spectator. It should not behave like the production's marketer, screenwriter, development executive, or studio.

## 16. Publication integrity

The final public article must not expose internal NCS governance or workflow vocabulary.

Follow `Governance/ncs-publication-integrity-standard.md`.

The exact hard-leak phrase list should be checked deterministically before review and again before delivery.

Public prose should not visibly perform the checklist.

Recurring motifs should not be underlined across every section merely because they are memorable.

## 17. Clean writer context

The Edition Writer should normally see:

- canon bible
- casting plan
- edition plan
- Brand Constitution
- Voice Constitution
- Editorial Anatomy
- Television Editorial Standard when relevant
- Relationship Story Standard when relevant
- Publication Integrity Standard
- approved Gold examples

It should not receive Ideation scores, duplicate audits, discarded concepts, Challenger notes, old development alternatives, or broad catalog history.

## 18. Gold-standard calibration

Current approved film calibration:

- `Examples/Gold/film-01-the-tell.md`
- `Examples/Gold/film-02-clearance.md`

Gold examples teach craft and judgment. They are not story templates.

Television has dedicated governance but still needs a formally locked television Gold example.

## 19. Deterministic Pre-Review Diagnostics

Before Forensic Review, run noncreative diagnostics that make likely regressions visible without rewriting the draft.

At minimum record:

- total paragraph count
- count of non-dialogue one-sentence paragraphs
- longest consecutive run of non-dialogue one-sentence paragraphs
- locations of those runs when practical
- em dash presence
- blank draft warning
- approximate word count
- obvious backstage technology language
- hard internal editorial leak matches
- required-heading presence for the resolved format
- section-overlap diagnostic

For FILM, compare THE MOVIE and THE SCENES for likely repeated close treatment.

For SERIES and LIMITED_SERIES, compare THE SEASON and THE EPISODES.

The overlap check is heuristic evidence for the Forensic Editor, not an automatic rewrite rule.

## 20. Forensic editorial review

The Forensic Editor reviews in this order:

1. production
2. genre execution
3. edition strategy and format execution
4. public integrity
5. prose
6. surface issues

It should explicitly inspect unresolved-value protection, genre proof, relationship delivery when relevant, scene ownership, ordinary world detail, spectatorship, character copy, casting voice, narrator frame, interpretation leakage, public internal-language leakage, motif overuse, and paragraph rhythm.

The review returns:

- one holistic `overall_score`
- `genre_specific_assessment`
- `scene_overlap_flags`
- revision requirements
- one `revision_route`

## 21. Score and route consistency

Follow `Governance/ncs-editorial-scoring-standard.md`.

A score below 8.0 means meaningful weaknesses remain.

Therefore:

- `overall_score < 8.0` may not use `revision_route = NONE`
- `overall_score >= 8.0` may still require revision when a material issue remains

The workflow must not manipulate the score to justify completion.

## 22. Revision routes

The Forensic Editor chooses the shallowest route that can solve the problem:

- `NONE`
- `PROSE`
- `EDITION`
- `CANON`

Limit automated deep rescue to one reentry cycle.

Typical routing:

- duplicate substantial scene treatment across sections: EDITION
- public romance under-demonstrated but canon contains stronger material: EDITION
- missing breakup logic, weak chemistry, or one-sided stakes in canon: CANON
- motif overload isolated to prose: PROSE or EDITION depending on scope
- internal terminology leakage: PROSE unless architecture caused it

## 23. Revision philosophy

Preserve what works. Do not reflexively rewrite every sentence.

When review flags duplicate scene treatment, choose one primary home and replace or compress the duplicate rather than paraphrasing the same scene twice.

When review flags internal terminology, rewrite from the audience side.

When review flags motif overuse, keep the strongest recurrences and remove redundant underlining.

When review flags THE FINISH as callback inventory, keep only the live pressures and objects that genuinely matter at the final threshold.

## 24. Final Forensic Review

The NCS score delivered to the Sheet and Google Doc must belong to the exact article delivered.

If any revision changes article text, run the Forensic Editor again.

The exact final draft must receive:

- valid numeric `overall_score`
- `overall_score >= 8.0`
- `revision_route = NONE`

A missing, stale, or sub-floor score blocks automatic delivery.

If the allowed automated path ends below 8.0 or still requests revision, stop without overwriting a previous successful draft.

## 25. Deterministic Final Article QA

After the exact final review, hard-check:

- nonblank final article
- nonblank final title
- valid exact-draft `overall_score >= 8.0`
- final `revision_route = NONE`
- no em dash character
- no backstage technology references
- no hard internal editorial leakage
- correct required headings for resolved format
- plausible word count
- valid final JSON objects

Also retain paragraph-rhythm and section-overlap diagnostics.

If a hard gate fails, do not deliver to Drive and do not set `DRAFTED`.

## 26. Social Asset Packet Builder

Once final article text, canon, and score are stable, create `ig_packet_json` using the current social and visual governance, prompt, and schema.

The packet always contains exactly three slide objects plus one short social `caption`.

The asset packet must reflect final canon, not the source Ideation premise.

The structured `format` value must exactly match one of:

- `FILM`
- `SERIES`
- `LIMITED_SERIES`

Billing blocks must not expose campaign-making or invent unnecessary participation claims.

## 27. Social Packet QA

Validate the exact final object that will be persisted after all repairs, normalization, mapping, or transformation.

Hard-check:

- current schema passes
- version is `ncs_ig_v1`
- title matches final title
- format is the exact resolved uppercase enum
- exactly three locked slide objects
- Slide 2 header is not a generic label such as `The Premise`
- Slide 2 body is nonblank
- Slide 3 newsletter line is exactly `THE FULL STORY IN NEVER COMING SOON`
- Slide 3 CTA is exactly `LINK IN BIO`
- Hollywood line is nonblank
- caption is nonblank and distinct from Slide 2 body copy
- no em dash character in public packet copy
- no hard internal terminology
- no backstage technology language
- billing block contains no false production-process credit

If packet QA fails, allow one packet-only repair, then revalidate the exact repaired object.

A schema-invalid final packet blocks delivery.

## 28. Drive delivery and final Ideas state

Only after final article QA, score, and social packet QA pass should the workflow write the final Google Doc.

The document begins:

`NCS SCORE: X.X / 10`

Then a blank line, then the final article only.

After successful Drive persistence, update the same Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` is the final success-state write.

Generation never sets `PUBLISHED` and never populates `published_url`.

## 29. Failure and force-redevelopment recovery

Before setting `GENERATING`, store `pre_generation_status`.

If the run fails or fails final quality gates before successful delivery, restore the exact row to `pre_generation_status` only if it is still `GENERATING`.

For a normal new run, that means DEVELOPMENT_SELECT.

For a failed force redevelopment of an existing successful draft, that means DRAFTED while preserving the prior successful final fields and Drive artifact.

The error workflow must recover the exact `idea_id`. Never reset an arbitrary GENERATING row.

## 30. Human review and publication

Generation ends at `DRAFTED` only after the automated quality floor is cleared.

The human editor remains the final publication authority.

A later human or publishing workflow may move:

`DRAFTED` -> `PUBLISHED`
