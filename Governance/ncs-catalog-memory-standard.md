# Never Coming Soon
## Catalog Memory and Deduplication Standard v1.2

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

Shared structural devices are also reusable. Two productions may both be real-time pressure cookers, countdown stories, odd-couple pairings, second-chance romances, workplace ensembles, or stories under public scrutiny without being duplicates.

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

But a shared abstract template is not enough. If the new arena materially changes what people do, what choices hurt, what scenes occur, what relationship drives the story, or why the audience cares, the concept may be genuinely distinct.

## 7. Duplicate statuses

Use three judgments:

### CLEAR

Materially distinct enough to continue.

### OVERLAP

Contains meaningful similarities worth noting, but may still be worth developing.

Overlap never automatically rejects a concept.

### DUPLICATE

The concept materially recreates an existing Never Coming Soon idea or a stronger idea in the same run such that developing both would add little creative value.

The `DUPLICATE` bar is intentionally high because it is a blocking status.

When two concepts plausibly deserve to exist as separate movies or shows, use `OVERLAP` or `CLEAR` rather than blocking one merely because an abstract description makes them rhyme.

## 8. Same-run duplication

Duplicate control must compare new concepts against one another, not only against historical rows.

Parallel creative rooms can independently discover the same hidden template.

When two new concepts are materially duplicates:

- keep the version with the stronger creative kernel and greater fertility
- mark the weaker version `DUPLICATE`
- do not mark both duplicate unless both also materially duplicate a prior catalog concept
- explain which idea is the canonical survivor

This is one of the most important anti-convergence checks in the workflow.

Same-run comparison is always between different `idea_id` values.

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

An exact fingerprint match only matters when it belongs to a different idea ID. A row matching its own fingerprint is not duplicate evidence.

## 11. Semantic duplicate judgment

A semantic audit should compare the candidate against prior concepts and same-run neighbors and answer:

- what is similar?
- what is materially different?
- is the similarity merely genre convention or a reusable dramatic device?
- does the candidate offer a different emotional engine?
- does the setup generate meaningfully different scenes?
- does the central relationship behave differently?
- do the major choices and consequences differ?
- would both productions plausibly deserve to exist in the same catalog?

When the compressed historical memory fits comfortably inside the model context, prefer complete catalog coverage over brittle keyword-only retrieval. A semantic reskin can use different vocabulary and still be the same dramatic machine.

When the catalog becomes too large for complete comparison, use staged retrieval or chunked audit. Retrieval may use genre, arena, signature fields, premise language, kernel language, status, and recency, but it should not rely on token overlap alone.

The answer should be editorial, not mathematical theater.

A useful blocking test is:

**Could you preserve most major scenes, the central relationship, conflict progression, and emotional destination by swapping nouns and surface details?**

If yes, `DUPLICATE` may be appropriate.

If not, meaningful resemblance is usually `OVERLAP`, not duplication.

## 12. Hard self-exclusion invariant

A candidate can never be compared against itself as historical evidence.

Before every duplicate audit, orchestration should exclude every current candidate `idea_id` from the historical-memory set used to judge that same candidate. The same exclusion applies to exact fingerprint collision lists.

The current candidate batch is already supplied separately for same-run comparison. Historical memory should represent prior catalog ideas, not a second copy of the candidates being judged.

Hard rules:

- `duplicate_of` must never equal the candidate's own `idea_id`
- `similar_ideas` must never include the candidate's own `idea_id`
- an exact fingerprint collision with the same `idea_id` must be discarded
- if a persisted current-run row appears in historical memory because the workflow wrote it before auditing, ignore it
- current-run concepts may still be compared with one another, but only across different IDs

A self-match is a workflow assembly defect, not a creative judgment.

If a duplicate-audit result violates this invariant, treat the result as invalid and repair or rerun the audit before any `DUPLICATE` status is allowed to block downstream development.

## 13. Two-pass duplicate protection

A seed can change during light expansion.

Therefore:

1. run a semantic duplicate audit on raw seeds before spending development effort
2. regenerate the memory signature and fingerprint after Idea Expansion
3. run a second semantic audit on expanded concepts before Development Select

The second pass should catch concepts whose richer development reveals a collision that was not visible in the raw premise.

Both passes must obey the self-exclusion invariant.

## 14. Creative grooves

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

## 15. Historical status matters

Not every historical row should exert the same blocking force.

Use status as context when judging a near-duplicate:

- `DEVELOPMENT_SELECT` or published work is strong canonical memory. A new surface reskin should usually be `DUPLICATE`.
- `DEVELOP` and `PROMISING` are meaningful prior claims, but a clearly stronger transformation may survive as `OVERLAP` when it opens materially better human or story possibilities.
- `HOLD` is weak memory. It should inform comparison but should not automatically prevent a substantially better version of the underlying material from advancing.
- a historical `DUPLICATE` row should never become the canonical blocker for a new concept. Compare against the concept it duplicated when available.

Catalog memory exists to prevent accidental repetition, not to grant permanent ownership to a weak early draft.

## 16. Slate awareness

The Ideation Director should review recent `DEVELOP`, `DEVELOPMENT_SELECT`, and any published concepts available to the workflow and summarize creative drift.

`PROMISING` ideas may inform memory when useful.

`HOLD` ideas should generally have less influence on slate drift because they have not earned the same creative weight.

The summary may influence the mix of ideation modes or provocations.

It should never convert into rigid quotas.

Good language:

"Recent concepts lean dark and institution-heavy. Explore more warmth and relationship-first ideas, but do not avoid a dark concept if it is exceptional."

Bad language:

"Generate exactly three comedies and two family films because the slate needs balance."

## 17. Historical inventory

`PROMISING` and `HOLD` ideas remain useful creative inventory.

They should not be treated as dead.

A future ideation run may revisit them when:

- the slate context changes
- a new angle emerges
- a better character appears
- a current event or discovery unlocks the idea
- Generation needs an alternative concept

## 18. Human notes

Human notes are high-authority memory.

If a human note explicitly says an idea should not be repeated, should be revisited later, or resembles another concept, that context should be respected when relevant.

Human notes should not be sprayed into every creative prompt. Retrieve them only when they materially affect the concept being compared or the run direction.

## 19. Casting repetition

Ideation does not need a casting registry.

If provisional casting ever appears in concept notes, actor repetition is not a primary duplicate signal.

Repeated actor usage is a downstream quality issue.

## 20. Title duplication

A repeated or highly similar title should be flagged, but title conflict does not mean concept duplication.

Working titles are provisional.

## 21. Human override

Human judgment can override duplicate status.

A concept marked `DUPLICATE` by the agent may be retained if the human editor believes the emotional engine or execution opportunity is meaningfully different.

Likewise, a technically `CLEAR` concept may be held if it simply feels too familiar to recent work.

## 22. Final memory test

The catalog memory system is working when it makes the next batch more imaginative without making the ideation engine timid.
