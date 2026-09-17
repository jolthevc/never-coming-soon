# Never Coming Soon
## Pre-Review Deterministic Diagnostics v1.0

## 1. Purpose

This spec defines the cheap deterministic diagnostics that run after Edition Writer and before Forensic Editor.

The diagnostics exist to surface likely mechanical drafting failures that are easy to miss in a cold read. They are not creative rules, automatic rewrite triggers, or proof that a draft is bad.

Core principle:

**Diagnostics produce leads. Forensic Editor decides whether the lead is a real problem.**

Do not auto-rewrite public prose from these signals. Do not block delivery for a soft warning by itself.

## 2. Preserve creative asymmetry

The diagnostic layer should be conservative.

It may flag:

- literal backstage or spoiler-management language
- probable section duplication
- repeated distinctive phrasing or tactic reuse
- suspicious episode-resolution repetition
- motif / phrase saturation
- structural omissions
- obvious rhythm anomalies

It should not decide:

- whether a scene is emotionally successful
- whether a character needs more conflict
- whether a compromise is too neat
- whether a romance has enough heat
- whether a finale needs more pressure
- whether a concept should be redeveloped

Those remain editorial judgments.

## 3. Hard and soft signal classes

### Hard signals

These are objective or near-objective checks:

- blank article
- missing required headings
- em dash character present
- malformed output
- missing title
- explicit internal workflow terminology
- literal public-copy leakage that clearly describes article construction rather than the fictional production

Hard signals may feed Final QA when current governance says they are blocking.

### Soft signals

These are warnings only:

- section overlap
- repeated distinctive phrase or object across sections
- repeated episode-resolution vocabulary
- sentence-rhythm anomalies
- generic review scaffolding
- possible spoiler-management phrasing that could also describe a real formal cut

Soft signals must be confirmed by Forensic Editor against the actual article.

## 4. Backstage / spoiler-management phrase warnings

Case-insensitively flag likely public-copy leakage when the phrase occurs outside quoted dialogue.

Priority phrase families include:

- `the show protects`
- `the movie protects`
- `we protect this`
- `the article protects`
- `the article withholds`
- `the reader should`
- `the button is`
- `the episode button`
- `hold the last`
- `hold this beat`
- `we cut`
- `cut on`
- `we stop here`
- `we leave the`
- `the image is enough`
- `the reveal stays protected`
- `the payoff stays protected`
- `spoiler protection`
- `unresolved value`
- `scene ownership`
- `edition plan`
- `development packet`
- `canon bible`
- `revision route`
- `public integrity`
- `proof of existence`
- `genre proof`
- `motif budget`

Also warn on phrases such as `cut to black` or `the episode ends on` only as soft signals, because those can describe a genuine formal feature of the fictional production.

Do not treat every use of words like `cut`, `protect`, `button`, `reader`, or `episode` as a failure. Prefer phrase-level matching over isolated keywords.

## 5. Required section checks

For FILM require, in order:

- THE PITCH
- THE CHARACTERS
- THE DREAM CAST
- THE MOVIE
- THE SCENES
- THE FINISH

For SERIES / LIMITED_SERIES require, in order:

- THE PITCH
- THE WORLD
- THE CHARACTERS
- THE DREAM CAST
- THE SEASON
- THE EPISODES
- THE FINISH

Report missing, duplicated, or badly reordered headings.

## 6. Section-overlap diagnostics

Normalize sections for comparison by:

- lowercasing
- removing markdown syntax
- removing punctuation
- collapsing whitespace
- excluding common stop words for distinctive-phrase analysis

Use overlap as a warning, not a verdict.

### Highest-priority comparisons

For FILM:

1. THE MOVIE vs THE SCENES
2. THE CHARACTERS vs THE DREAM CAST
3. THE SCENES vs THE FINISH
4. THE PITCH vs later signature-scene sections when the same distinctive phrase or physical mechanism recurs

For SERIES / LIMITED_SERIES:

1. THE SEASON vs THE EPISODES
2. THE CHARACTERS vs THE DREAM CAST
3. THE EPISODES vs THE FINISH
4. THE WORLD vs THE SEASON when world rules are being restated as season movement

### Useful heuristic signals

Flag when one or more of the following occur:

- a distinctive 4+ word phrase repeats across two major sections
- several distinctive 2-3 word noun phrases repeat across the same section pair
- the same quoted line appears in multiple sections
- the same unusual prop, tactic, ritual, rule, or physical action appears with highly similar surrounding language in multiple sections
- normalized token overlap between two long sections is unusually high relative to article-wide overlap

Do not flag normal recurrence of character names, title words, format terms, common locations, or unavoidable premise nouns by themselves.

The output should identify the section pair and the repeated phrase / object / tactic so Forensic Editor can verify whether the article actually restages the same material.

## 7. Character vs Dream Cast reuse

Run a dedicated comparison between THE CHARACTERS and THE DREAM CAST.

Warn when a character's Dream Cast paragraph substantially reuses the same:

- prop
- gesture
- line
- anecdote
- scene outcome
- signature behavior
- compressed character label

The goal is not to prevent any overlap. Casting naturally refers to the same person. The warning is for cases where the casting paragraph simply performs the character card again with an actor attached.

## 8. Television Season vs Episodes reuse

For SERIES / LIMITED_SERIES, compare THE SEASON and THE EPISODES at both phrase and event level when practical.

Warn when multiple episode capsules appear to restage events already described concretely in THE SEASON, especially when the same action, outcome, or distinctive object is repeated.

THE SEASON may name broad developments that THE EPISODES later dramatizes. Do not flag a shared topic alone.

The warning should focus on duplicated staging, not duplicated subject matter.

## 9. Episode-resolution repetition warning

For television, parse episode capsules when possible and inspect their final sentence or final approximately 25% of text.

Generate a soft warning when a strong majority of selected capsules repeatedly resolve through the same vocabulary or institutional outcome, especially clusters such as:

- `rule`
- `policy`
- `protocol`
- `standard`
- `pledge`
- `charter`
- `procedure`
- `system`
- `training`
- `lesson`
- `learns`
- `realizes`
- `agreement`
- `compromise`
- `solution`

This is only a pattern alert. A show may legitimately be about policy or systems. Forensic Editor must decide whether the repeated resolution shape actually makes the episodes feel interchangeable.

Do not infer a creative problem from one or two matches.

## 10. Repeated motif / phrase saturation

Across the full article, surface unusually repeated distinctive phrases, metaphors, props, rituals, or named mechanisms.

Useful warnings include:

- the same unusual noun phrase appearing in 3+ public sections
- one metaphor family dominating several sections
- one signature tactic appearing as the main evidence in Character, Dream Cast, plot, and Finish sections
- one branded ritual or object receiving repeated explanatory attention

Exclude ordinary names and unavoidable premise vocabulary.

Again, recurrence can be intentional. This diagnostic only asks Forensic Editor to inspect whether repetition is earning new story or pressure.

## 11. Rhythm diagnostics

Retain existing cheap rhythm checks:

- consecutive one-sentence paragraphs
- clusters of very short declarative sentences
- clusters of unusually long sentences
- repeated paragraph-open constructions when detectable

Do not turn these into sentence-length quotas.

The goal is to spot obvious machine-like regularity, not normalize prose.

## 12. Suggested diagnostic object

The orchestration layer may keep its existing JSON shape. If convenient, organize warnings conceptually as:

- `hard_integrity_flags`
- `backstage_phrase_flags`
- `required_heading_flags`
- `section_overlap_flags`
- `character_cast_reuse_flags`
- `tv_season_episode_reuse_flags`
- `episode_resolution_pattern_flags`
- `motif_phrase_saturation_flags`
- `rhythm_flags`
- `summary`

Do not require a schema migration merely to adopt this spec if the existing diagnostic object can carry equivalent information.

## 13. Forensic handoff

Pass the exact diagnostic object to Forensic Editor with the exact article.

Forensic Editor should:

1. verify a warning against the article before treating it as a problem
2. ignore false positives
3. identify the strongest 2 to 4 things that should not be damaged before recommending changes
4. distinguish public-copy mechanics from canon problems
5. avoid routing to revision merely because diagnostics contain warnings

## 14. Final posture

The diagnostic layer should get better at catching mechanical mistakes without becoming another creative author.

A good implementation produces fewer useful warnings, not more warnings for their own sake.

**Catch leaks and repetition. Preserve taste for the editor.**
