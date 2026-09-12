# Never Coming Soon
## Directed Ideation Standard v1.0

## 1. Purpose

This standard governs human-directed creative input into the Never Coming Soon Ideation system.

The dashboard may receive anything from one word such as `dog` to a detailed concept brief. The system should use that direction without confusing human intent with a finished story.

Directed Ideation changes the territory being explored. It does not lower the creative quality bar.

## 2. Three entry modes

### GENERAL

Normal broad Ideation. No creative direction is required. The Ideation Director designs a varied session using catalog awareness and the existing Ideation Constitution.

### DIRECTED

Explore a human-supplied territory and return a field of materially different concepts inside it.

Examples of direction may be sparse (`dog`, `Las Vegas`, `marriage`) or detailed. Optional constraints may narrow format, genre, or starting mode.

The workflow still uses multiple creative rooms unless the human explicitly requests one mode.

### CONCEPT_INTAKE

Take one specific human or dashboard-spark concept and admit that exact concept into the normal NCS evaluation pipeline.

Concept Intake is not a request for thirty related ideas. It is a request to preserve the concept's identity, structure it as one canonical seed, then subject it to duplicate audit, curation, expansion, and Development Selection.

## 3. Constraint hierarchy

Human-supplied structured constraints have this authority order:

1. explicit concept text
2. explicit format constraint
3. explicit genre constraint
4. explicit ideation-mode preference
5. anchor strength
6. normal Director/catalog steering

Catalog awareness may improve originality inside the brief. It should not redirect the system away from explicit human direction.

## 4. Anchor strength

### LOOSE

The human direction is inspiration. Concepts may move significantly away from the literal noun or phrase if the creative connection remains intelligible.

Example: `dog` may produce concepts about pet custody, dog-show culture, animal-control work, or a relationship formed through a lost dog.

### CENTERED

Default. The human direction should materially drive the premise, world, relationship, or story engine. It should not be decorative.

### STRICT

The human direction is a hard concept requirement. Every returned seed must fundamentally depend on it.

Anchor strength does not mean quality tolerance. Weak concepts should still be discarded and replaced.

## 5. Format constraints

Allowed dashboard values:

- `ANY`
- `FILM`
- `SERIES`
- `LIMITED_SERIES`

`ANY` preserves normal format judgment.

When a concrete format is supplied, all Directed Ideation seeds must use it. Do not reinterpret a format constraint as a preference.

Concept Intake may normalize a user's implied format only when none was specified.

## 6. Genre constraints

Genre is a human-facing creative constraint, not a closed taxonomy.

Dashboard controls may offer common genres for convenience, while still permitting free-text genre direction.

When a genre is explicitly supplied, it should be the primary genre promise of the returned concepts. Secondary tones and genres may vary when they strengthen the idea.

Do not satisfy `horror` by attaching superficial horror furniture to a concept whose actual engine is unrelated.

## 7. Ideation mode controls

The dashboard may expose `AUTO` plus the existing Ideation modes:

- character_first
- relationship_first
- world_first
- situation_first
- genre_first
- scene_first
- ending_first
- truth_first
- discovery_first
- title_first

`AUTO` lets the Ideation Director allocate several modes normally.

A selected mode means the run should begin from that creative method. It does not require every concept to share one structure or gimmick.

## 8. Sparse direction

Sparse human direction should not trigger clarifying questions unless the request is genuinely uninterpretable.

`dog` is enough to run Directed Ideation.

The Director should interpret sparse direction expansively according to anchor strength while respecting any structured constraints.

Do not pad sparse direction with invented user intent such as presumed tone, audience, theme, setting, or demographic.

## 9. Detailed direction

When the human provides a fleshed-out idea, preserve the elements that appear intentional while keeping provisional details provisional.

Do not reward length by treating every sentence as canon.

If the user clearly describes one specific concept and wants that concept developed rather than a field around it, route to CONCEPT_INTAKE rather than DIRECTED.

## 10. Concept Intake preservation

Concept Intake should make the smallest transformation needed to produce a valid NCS seed.

It may:

- supply a temporary working title
- infer format when absent
- infer primary genre when absent
- articulate a creative kernel
- articulate why the concept is exciting
- identify a few initial possibilities
- create creative tags and normalized signature

It must not:

- add a twist to make the idea seem cleverer
- add trauma, conspiracy, murder, mythology, or prestige machinery without necessity
- replace the central relationship or situation
- rewrite a sparse concept into a different movie
- pretend uncertain details were supplied by the human

Concept Intake structures first. The existing Expander and Generation stages own later invention.

## 11. Dashboard Quick Sparks

Quick Sparks are disposable inspiration, not canonical Ideation output.

They should be one-sentence entertainment concepts with immediate screen life. Their job is to make the human editor say, `that one`.

Quick Sparks:

- are not written to Sheets
- receive no permanent idea_id
- do not enter catalog memory
- may use a cheaper model than canonical Ideation
- should be generated in batches of five
- should remain concise enough to scan in seconds

When the human chooses a Quick Spark, send its exact concept sentence into CONCEPT_INTAKE.

Do not route the selected Spark into a thirty-seed Directed run unless the human explicitly asks to explore the broader territory instead.

## 12. Quality principle

Directed control should reduce search space, not reduce imagination.

The governing rule remains:

**Find something we cannot stop imagining, inside the territory the human actually asked to explore.**
