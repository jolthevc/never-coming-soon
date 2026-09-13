# Never Coming Soon
## Generation, Review and Revision Operating System v2.3

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
2. Set `GENERATING`
3. Production Development
4. Optional Grounding Research
5. Independent Story Challenge
6. Canon Building
7. Canon Freeze
8. Casting
9. Edition Architecture
10. Style Calibration Load
11. Edition Drafting
12. Deterministic Pre-Review Diagnostics
13. Forensic Editorial Review
14. Targeted Revision or Controlled Rescue
15. Final Forensic Review of the exact final draft
16. Deterministic Final QA
17. Social Asset Packet Builder
18. Social Packet QA
19. Google Drive Delivery
20. Final Ideas Row Update to `DRAFTED`

Do not collapse these into one giant prompt.

## 6. Production development before prose

The Production Developer creates the first serious version of the movie or show.

Development should establish causality, character agency, relationships, escalating pressure, genre delivery, an earned ending, ordinary world texture, and enough specific scenes to expose whether the production actually works.

Development should also ask whether the production naturally supports any higher-ceiling image, set piece, reversal, formal idea, comic construction, or collision that could make a viewer suddenly need the rest.

This is not a twist requirement.

## 7. Genre proof and Contained Proof candidates

Development should create enough actual genre pleasure that the public edition can prove the product without spending its decisive value.

A useful internal instrument is the **Contained Proof**: a lower-stakes sequence that fully demonstrates the production's primary engine or genre pleasure without spending major unresolved value.

It demonstrates capability, not stakes, and should usually end smaller than it began.

Contained Proofs are optional.

The phrase `Contained Proof` is internal terminology and must never appear as a public article label.

## 8. Selective research

Research is conditional. Use it when real-world accuracy materially improves the production.

Research should reveal dramatic texture and constraints rather than accumulate trivia.

## 9. Independent challenge

The Story Challenger should identify dead story sections, generic character functions, weak causality, false complexity, derivative patterns, unearned endings, wrong format, missed opportunities, weak genre delivery, and obvious unused higher-ceiling invention.

The Challenger diagnoses. It does not own the replacement story.

## 10. Canon Builder and Canon Freeze

After the Canon Builder, the production should meaningfully exist.

For film, canon should know the characters, relationships, world, complete story trajectory, actual ending, emotional resolution, signature scenes, tone, genre delivery, ordinary world texture, major unresolved public value, genre pleasures available for public demonstration, and possible Contained Proof candidates.

For series, canon should also know the recurring engine, Season One spine, major character movement, concrete episode purposes, actual Season One finale, and future engine when relevant.

For limited series, canon should know the contained ending and chapter logic.

Once frozen, downstream agents represent the production rather than casually reinventing it.

## 11. Casting after canon

Dream casting happens after characters exist.

Internal casting logic may reason in terms of what the role needs. Public copy later should sound as though the performance has already been watched.

## 12. Edition architecture before drafting

The Edition Architect decides how the reader should discover the production.

It chooses the opening strategy, public character focus, canonical section flow, compression, slow-down points, signature scenes, unresolved value, genre demonstrations, ordinary world detail, spectatorship, genre-specific rhythm, recurring-motif budget, and Finish convergence.

For film, use:

- THE PITCH
- THE CHARACTERS
- THE CAST
- THE MOVIE
- THE SCENES
- THE FINISH

For television, follow `Governance/ncs-television-editorial-standard.md`.

Internal planning concepts must be translated into audience-side prose downstream. Do not plan public labels such as `Contained Proof`, `extractable play`, `results stay protected`, or `the spine is simple`.

## 13. Public selection doctrine

Protect whatever carries the production's major unresolved value. Give away enough genre pleasure to prove the production delivers.

When those overlap, a lower-stakes demonstration may prove capability without spending the stake.

Do not protect the production so aggressively that the article becomes vague.

## 14. Interpretation and narrator discipline

**Do not state meaning, mechanism, or consequence that the material is capable of delivering on its own.**

This applies to theme and plot mechanics alike.

The narrator may behave like a spectator. It should not behave like the production's marketer, screenwriter, development executive, or studio.

## 15. Publication integrity

The final public article must not expose internal NCS governance or workflow vocabulary.

Follow `Governance/ncs-publication-integrity-standard.md`.

The exact hard-leak phrase list should be checked deterministically before review and again before delivery.

Public prose should not visibly perform the checklist. Ordinary world detail should feel discovered rather than curated for compliance.

Recurring motifs should not be underlined across every section merely because they are memorable.

## 16. Clean writer context

The Edition Writer should normally see:

- canon bible
- casting plan
- edition plan
- Brand Constitution
- Voice Constitution
- Editorial Anatomy
- Television Editorial Standard when relevant
- Publication Integrity Standard
- approved Gold examples

It should not receive Ideation scores, duplicate audits, discarded concepts, Challenger notes, old development alternatives, or broad catalog history.

## 17. Gold-standard calibration

Current approved film calibration:

- `Examples/Gold/film-01-the-tell.md`
- `Examples/Gold/film-02-clearance.md`

Gold examples teach craft and judgment. They are not story templates.

Television has dedicated governance but still needs a formally locked television Gold example.

## 18. Deterministic Pre-Review Diagnostics

Before Forensic Review, run noncreative diagnostics that make likely regressions visible without rewriting the draft.

At minimum record:

- total paragraph count
- count of non-dialogue one-sentence paragraphs
- longest consecutive run of non-dialogue one-sentence paragraphs
- locations or paragraph indices of those runs when practical
- em dash presence
- blank draft warning
- approximate word count
- obvious backstage technology language
- hard internal editorial leak matches from Publication Integrity Standard
- required-heading presence for the resolved format

Paragraph diagnostics are warnings, not automatic failures.

Hard internal terminology and backstage technology leakage should normally require revision before delivery.

## 19. Forensic editorial review

The Forensic Editor reviews in this order:

1. production
2. edition strategy and format execution
3. public integrity
4. prose
5. surface issues

It should explicitly inspect unresolved-value protection, genre-pleasure proof, proof of existence, evidence of spectatorship, character copy, casting voice, section-role integrity, narrator frame, interpretation leakage, public internal-language leakage, motif overuse, and paragraph rhythm.

For television, it should also review THE WORLD, public ensemble focus, THE SEASON, episode-selection strategy, episode-capsule voice, and season Finish behavior.

The review returns one holistic `overall_score` from 1.0 to 10.0 for the exact draft in that call.

## 20. Revision routes

The Forensic Editor chooses the shallowest route that can solve the problem:

- `NONE`
- `PROSE`
- `EDITION`
- `CANON`

Limit automated deep rescue to one reentry cycle.

A missing twist or startling moment is not automatically a CANON problem.

## 21. Revision philosophy

Preserve what works. Do not reflexively rewrite every sentence.

When review flags habitual one-line paragraph rhythm, actively recombine routine transitional paragraphs while preserving isolation that genuinely creates force, suspense, comedy, or camera-like emphasis.

When review flags internal terminology, rewrite the passage from the audience side rather than merely deleting the forbidden phrase.

When review flags motif overuse, keep the strongest recurrences and remove redundant underlining.

## 22. Final Forensic Review is mandatory after text changes

The NCS score delivered to the Sheet and Google Doc must belong to the exact article that is delivered.

Therefore:

- if the first review returns `NONE` and no article text changes afterward, that review may serve as the final review
- if any revision changes article text, run the Forensic Editor again on the revised draft
- if an EDITION or CANON rescue causes a new draft, review the resulting exact final draft again
- never carry forward a score from an earlier draft after text changed

A missing, nonnumeric, or stale `overall_score` blocks final delivery.

Do not write `N/A` as a substitute.

## 23. Deterministic Final QA

After the exact final review, run final checks for:

- nonblank final article
- nonblank final title
- valid numeric `overall_score` from 1.0 to 10.0
- no em dash character
- no backstage technology references
- no hard internal editorial leakage
- correct required headings for resolved format
- plausible word count
- major-name consistency where checkable
- episode-count consistency where checkable
- paragraph-rhythm diagnostics

If a hard gate fails, do not deliver to Drive and do not set `DRAFTED`.

## 24. Social Asset Packet Builder

Once final article text, canon, and score are stable, create `ig_packet_json` using:

- `Governance/ncs-visual-constitution.md`
- `Governance/ncs-social-asset-standard.md`
- `Prompts/Generation/10-ig-asset-packet-builder.system.md`
- `Schemas/ig-asset-packet.schema.json`

The packet always contains exactly three slide objects plus one short social `caption`.

The asset packet must reflect final canon, not the source Ideation premise.

Slide 2 copy must be exact placement-ready copy.

Slide 3 Hollywood line must be an NCS/Hollywood personality beat, not a second thematic production tagline.

## 25. Social Packet QA

Before persistence, deterministically check:

- valid schema
- title matches final title
- exactly three locked slide objects
- `version = ncs_ig_v1`
- Slide 1 contains required content slots
- Slide 2 header is not a generic label such as `The Premise`
- Slide 2 body is nonblank and free of em dashes
- Slide 3 newsletter line is exactly `THE FULL STORY IN NEVER COMING SOON`
- Slide 3 CTA is exactly `LINK IN BIO`
- Hollywood line is nonblank
- caption is nonblank
- no em dash character anywhere in public packet copy
- no hard internal terminology
- no backstage technology language

If social packet QA fails, repair the packet before final delivery.

Do not mutate the article merely because the social packet failed.

## 26. Drive delivery

Only after final article QA, final score, and social packet QA pass should the workflow write the final Google Doc.

The document begins:

`NCS SCORE: X.X / 10`

Then a blank line, then the final article only.

## 27. Final Ideas state

After successful Drive persistence, update the same Ideas row with:

- `final_title`
- `draft_url`
- `ncs_score`
- `ig_packet_json`
- `status = DRAFTED`

`DRAFTED` is the final success-state write.

Generation never sets `PUBLISHED` and never populates `published_url`.

## 28. Failure recovery

If Generation fails after setting `GENERATING` but before final delivery:

- never set `DRAFTED`
- never write a placeholder score
- reset the exact failed row to `DEVELOPMENT_SELECT` when appropriate
- preserve human notes
- preserve successful prior final artifacts during failed force redevelopment

A small companion n8n Error Trigger workflow is the preferred cleanup mechanism.

## 29. Human review and publication

Generation ends at `DRAFTED`.

The human editor remains the final publication authority.

A later human or publishing workflow may move:

`DRAFTED` -> `PUBLISHED`
