# Never Coming Soon
## Catalog Memory and Deduplication Standard v1.1

## 1. Purpose

Catalog memory exists to improve creativity over time.

It should help Never Coming Soon:

- avoid true duplicates
- catch surface reskins of the same underlying concept
- notice repeated creative grooves
- maintain a varied slate without quotas
- reuse old promising ideas intelligently
- prevent the system from forgetting what it has already considered

Catalog memory should create awareness, not fear.

It is not a quota engine.

## 2. Governing principle

**Remember enough to avoid accidental repetition. Stay free enough to repeat a genre, theme, or archetype when the new idea is genuinely good.**

## 3. Memory must not become an anchor

Historical context can improve originality or quietly destroy it.

Do not flood creative generators with the catalog.

The Ideation Director may see broad catalog context because it needs to identify drift and design the creative session.

The Seed Generator should not receive individual historical premises, titles, kernels, or pitches by default. The Director should translate memory into an open creative room, and semantic duplicate control should happen after generation.

This separation matters. Showing a creator the ideas it is supposed not to copy can make those same ideas more cognitively available and narrow the search space.

Human direction remains appropriate creative context because it is an intentional brief, not accidental catalog anchoring.

Catalog memory should function as negative space, not as an inspiration library to imitate.

## 4. What is not duplication

These are not duplicates by themselves:

- two sports movies
- two father-daughter stories
- two stories set in Chicago
- two courtroom thrillers
- two workplace comedies
- two productions about grief
- two stories with a washed-up protagonist

Genres, themes, arenas, settings, and archetypes are reusable materials.

## 5. What may be meaningful overlap

Overlap deserves attention when several structural elements recur together.

Example:

Concept A: A disgraced former NBA star returns home and reluctantly coaches his estranged daughter's high-school team.

Concept B: A washed-up professional golfer returns to his hometown and reluctantly coaches his estranged daughter's high-school team.

The sport differs, but the protagonist archetype, return-home mechanism, parent-child relationship, coaching engine, and emotional movement may be too similar.

The duplicate auditor should explain the overlap rather than relying on genre labels.

## 6. Surface reskins

Changing the nouns does not necessarily create a new idea.

Watch for concepts where the following remain essentially the same while the profession, sport, city, era, or gender changes:

- protagonist function
- central relationship
- inciting situation
- recurring engine
- main emotional movement
- source of conflict
- central dramatic question

A cosmetic reskin should not survive as a distinct concept merely because the setting is different.

## 7. Duplicate statuses

Use three judgments:

### CLEAR

Materially distinct enough to continue.

### OVERLAP

Contains meaningful similarities worth noting, but may still be worth developing.

Overlap never automatically rejects a concept.

### DUPLICATE

The concept materially recreates an existing Never Coming Soon idea or a stronger idea in the same run such that developing both would add little creative value.

## 8. Same-run duplication

Duplicate control must compare new concepts against one another, not only against historical rows.

Parallel creative rooms can independently discover the same hidden template.

When two new concepts are materially duplicates:

- keep the version with the stronger creative kernel and greater fertility
- mark the weaker version `DUPLICATE`
- do not mark both duplicate unless both also materially duplicate a prior catalog concept
- explain which idea is the canonical survivor

This is one of the most important anti-convergence checks in the workflow.

## 9. Structural memory signature

Every viable idea should receive one normalized structural signature containing approximately:

- format
- primary genre
- arena or world
- protagonist archetype
- central relationship
- core situation
- story engine
- setting when creatively important

The signature should be preserved inside `creative_tags_json` as `memory_signature` so later runs can retrieve and compare structure without adding another Sheet column.

The same signature is canonicalized and hashed by n8n into the `fingerprint` field.

The AI agent should not invent cryptographic hashes.

## 10. Fingerprints are exact-memory aids, not semantic truth

A SHA-256 fingerprint can identify identical canonical signatures.

It cannot measure creative similarity.

Do not use hash distance, prefix similarity, or other mathematical theater as a semantic signal.

Semantic duplicate judgment still requires comparison of the actual concept material.

## 11. Semantic duplicate judgment

A semantic audit should compare the candidate against prior concepts and same-run neighbors and answer:

- what is similar?
- what is materially different?
- is the similarity merely genre convention?
- does the candidate offer a different emotional engine?
- does the setup generate meaningfully different scenes?
- would both productions plausibly deserve to exist in the same catalog?

When the compressed historical memory fits comfortably inside the model context, prefer complete catalog coverage over brittle keyword-only retrieval. A semantic reskin can use different vocabulary and still be the same dramatic machine.

When the catalog becomes too large for complete comparison, use staged retrieval or chunked audit. Retrieval may use genre, arena, signature fields, premise language, kernel language, status, and recency, but it should not rely on token overlap alone.

The answer should be editorial, not mathematical theater.

## 12. Two-pass duplicate protection

A seed can change during light expansion.

Therefore:

1. run a semantic duplicate audit on raw seeds before spending development effort
2. regenerate the memory signature and fingerprint after Idea Expansion
3. run a second semantic audit on expanded concepts before Development Select

The second pass should catch concepts whose richer development reveals a collision that was not visible in the raw premise.

## 13. Creative grooves

Catalog memory should also notice repeated tendencies across recent ideas.

Examples:

- too many dark adult thrillers
- too many self-destructive male antiheroes
- too many secret institutions
- too many New York or Los Angeles settings
- too many conspiracy engines
- too many stories about elite wealth
- too many tragic endings
- too many historical productions
- too many stories that rely on a twist
- too many return-home redemption arcs
- too many dead-parent or inheritance catalysts

These observations inform ideation but do not impose bans.

If the best next idea is another thriller, make another thriller.

## 14. Historical status matters

Not every historical row should exert the same blocking force.

Use status as context when judging a near-duplicate:

- `DEVELOPMENT_SELECT` or published work is strong canonical memory. A new surface reskin should usually be `DUPLICATE`.
- `DEVELOP` and `PROMISING` are meaningful prior claims, but a clearly stronger transformation may survive as `OVERLAP` when it opens materially better human or story possibilities.
- `HOLD` is weak memory. It should inform comparison but should not automatically prevent a substantially better version of the underlying material from advancing.
- a historical `DUPLICATE` row should never become the canonical blocker for a new concept. Compare against the concept it duplicated when available.

Catalog memory exists to prevent accidental repetition, not to grant permanent ownership to a weak early draft.

## 15. Slate awareness

The Ideation Director should review recent `DEVELOP`, `DEVELOPMENT_SELECT`, and any published concepts available to the workflow and summarize creative drift.

`PROMISING` ideas may inform memory when useful.

`HOLD` ideas should generally have less influence on slate drift because they have not earned the same creative weight.

The summary may influence the mix of ideation modes or provocations.

It should never convert into rigid quotas.

Good language:

"Recent concepts lean dark and institution-heavy. Explore more warmth and relationship-first ideas, but do not avoid a dark concept if it is exceptional."

Bad language:

"Generate exactly three comedies and two family films because the slate needs balance."

## 16. Historical inventory

`PROMISING` and `HOLD` ideas remain useful creative inventory.

They should not be treated as dead.

A future ideation run may revisit them when:

- the slate context changes
- a new angle emerges
- a better character appears
- a current event or discovery unlocks the idea
- Generation needs an alternative concept

## 17. Human notes

Human notes are high-authority memory.

If a human note explicitly says an idea should not be repeated, should be revisited later, or resembles another concept, that context should be respected when relevant.

Human notes should not be sprayed into every creative prompt. Retrieve them only when they materially affect the concept being compared or the run direction.

## 18. Casting repetition

Ideation does not need a casting registry.

If provisional casting ever appears in concept notes, actor repetition is not a primary duplicate signal.

Repeated actor usage is a downstream quality issue.

## 19. Title duplication

A repeated or highly similar title should be flagged, but title conflict does not mean concept duplication.

Working titles are provisional.

## 20. Human override

Human judgment can override duplicate status.

A concept marked `DUPLICATE` by the agent may be retained if the human editor believes the emotional engine or execution opportunity is meaningfully different.

Likewise, a technically `CLEAR` concept may be held if it simply feels too familiar to recent work.

## 21. Final memory test

The catalog memory system is working when it makes the next batch more imaginative without making the ideation engine timid.
