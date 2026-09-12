# Never Coming Soon
## Directed Ideation Standard v1.0

## 1. Purpose

This standard governs user-directed creative input entering the Never Coming Soon Ideation system from an internal dashboard or equivalent control surface.

The goal is to let a human editor steer territory without turning Ideation into a literal prompt executor or collapsing creative range.

Directed input should improve focus while preserving surprise, fertility, originality, and Generation optionality.

## 2. Three run types

Ideation supports three distinct entry modes.

### GENERAL

No specific creative subject is required. The existing Ideation Director designs a broad creative session using catalog awareness and multiple ideation modes.

### DIRECTED

The human provides a subject, phrase, rough thought, or developed brief and may add structured constraints. DIRECTED means explore this territory seriously and return a field of strong concepts within the requested creative boundaries.

### CONCEPT_INTAKE

The human provides one specific concept they want evaluated and developed as that concept. The system normalizes the concept into the canonical seed contract, then enters the existing duplicate-audit, curation, expansion, and Development Select pipeline. It must not generate a fresh batch of unrelated concepts around the input.

## 3. Human direction hierarchy

Explicit human direction has high authority. Honor literal hard constraints exactly when supplied, especially required format, required genre, strict subject anchoring, and specific setting, character, relationship, or premise requirements clearly stated as requirements.

Do not over-literalize sparse input. A one-word direction is creative territory, not a command to make a batch of cosmetically identical stories.

## 4. Structured controls

The dashboard may supply:

- `run_type`: `GENERAL`, `DIRECTED`, or `CONCEPT_INTAKE`
- `target_seed_count`
- `human_direction`
- `target_format`: null, `FILM`, `SERIES`, or `LIMITED_SERIES`
- `target_genre`: nullable free text
- `preferred_ideation_mode`: nullable Ideation mode
- `anchor_strength`: `LOOSE`, `CENTERED`, or `STRICT`

Absent optional controls should not be invented by the orchestration layer.

## 5. Anchor strength

### LOOSE

Treat the direction as a source of creative energy. Concepts may approach it indirectly or through a surprising neighboring arena, but the connection should remain intelligible.

### CENTERED

Default for DIRECTED runs. Every concept should materially engage the supplied direction while retaining freedom over character, world, genre expression, scale, relationship, and dramatic engine unless otherwise constrained.

### STRICT

The supplied direction is structurally central. The core concept should stop working if that direction is removed.

## 6. Format and genre control

When `target_format` is supplied, every generated seed must use that format. When `target_genre` is supplied, treat it as the primary genre promise while still varying secondary genre, tone, scale, arena, and story engine.

When either is absent, retain the existing Ideation Constitution logic.

## 7. Preferred ideation mode

The editor may optionally request one existing starting mode: `character_first`, `relationship_first`, `world_first`, `situation_first`, `genre_first`, `scene_first`, `ending_first`, `truth_first`, `discovery_first`, or `title_first`.

When supplied, the Director should materially weight the session toward that mode without making every output structurally identical.

## 8. Sparse-query interpretation

Sparse direction is not low-value direction. Treat a short subject as creative territory worth interrogating through people, relationships, worlds, situations, genre pleasures, recurring engines, scenes, social truths, and visual identities.

Do not respond to sparse direction with generic category tropes.

## 9. Detailed-query interpretation

When a developed brief is supplied in DIRECTED mode, preserve its important creative intent rather than reducing it to disconnected keywords. If the input is already a specific concept the editor wants advanced, use CONCEPT_INTAKE instead.

## 10. Concept Intake preservation rule

CONCEPT_INTAKE is normalization, not replacement. Preserve the human concept's recognizable identity.

The intake step may clarify the premise, infer provisional format or genre when absent, identify the creative kernel, articulate why it is exciting, create initial possibilities, and create creative tags and a normalized signature. It may not quietly substitute a different premise because it believes it has a better idea.

Later Ideation stages retain their normal authority to critique, expand, or reject the concept.

## 11. Quick Sparks are not canonical Ideation

Dashboard Quick Sparks are disposable creative stimuli. They are not persisted automatically, do not receive idea IDs, do not enter catalog memory, do not count as duplicate-audited concepts, may use a cheaper model, and should be one sentence and immediately understandable.

A Quick Spark becomes canonical only when the editor explicitly chooses `Develop this`, at which point it enters CONCEPT_INTAKE.

## 12. Do not expose sampling physics as creative controls

The dashboard should not ask the editor to manage raw model settings such as temperature, top-p, token budget, or arbitrary randomness sliders. Human controls should describe creative intent. The orchestration layer owns model settings.

## 13. Dashboard defaults

Recommended defaults:

- DIRECTED target seed count: 12
- anchor strength: `CENTERED`
- target format: Any
- target genre: Any
- preferred ideation mode: Auto

GENERAL keeps the normal 30-seed default.

## 14. Final test

A directed run succeeds when the editor recognizes the territory they asked for while still encountering ideas they would not have written themselves.

A Concept Intake succeeds when the human's idea remains recognizably intact, but the canonical Ideation system has made it legible enough to judge and develop.