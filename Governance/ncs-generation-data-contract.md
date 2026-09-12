# Never Coming Soon
## Generation Data Contract v1.0

## 1. Purpose

This document defines the persistent state for the Generation workflow.

The architecture should remain lean.

One row equals one developed NCS production.

## 2. Spreadsheet

Use a separate native Google Spreadsheet named:

`Never Coming Soon - Productions`

Use one working tab:

`Productions`

Do not add additional tabs in v1 unless implementation pain demonstrates a real need.

## 3. Canonical columns

Use these 15 columns in this order:

1. `production_id`
2. `idea_id`
3. `created_at`
4. `status`
5. `development_packet_json`
6. `development_blueprint_json`
7. `research_packet_json`
8. `story_challenge_json`
9. `canon_bible_json`
10. `casting_json`
11. `edition_plan_json`
12. `draft_v1_markdown`
13. `editorial_review_json`
14. `draft_final_markdown`
15. `human_notes`

## 4. IDs

Default production ID derives from the Ideation ID.

Example:

`NCS-I-000185` -> `NCS-P-000185`

One production per selected idea is the v1 default.

Future explicit redevelopment variants may add a version suffix, but v1 should not invent variant logic prematurely.

## 5. Status values

Allowed values:

- `DEVELOPING`
- `RESEARCHING`
- `CHALLENGING`
- `CANON_READY`
- `CAST_READY`
- `EDITION_PLANNED`
- `DRAFTED`
- `REVIEWED`
- `READY_FOR_HUMAN_REVIEW`
- `HOLD`
- `ABANDONED`

Status changes must be explicit.

## 6. Field ownership

### Identity fields

`production_id`, `idea_id`, and `created_at` are immutable after creation.

### development_packet_json

A snapshot of the exact Development Packet used to begin Generation.

Do not silently replace it if the Ideation row later changes.

### development_blueprint_json

Output of the Production Developer.

### research_packet_json

Blank when no research was requested.

Contains the structured grounding packet when research ran.

### story_challenge_json

Independent critique of the development blueprint.

### canon_bible_json

The authoritative internal production after Canon Builder.

Downstream agents should represent this canon rather than casually reinvent it.

A controlled canon-reopen loop may replace this field if editorial review identifies a foundational production problem.

### casting_json

Dream casting plan created after canon exists.

### edition_plan_json

Public storytelling architecture created after canon and casting.

### draft_v1_markdown

First complete public NCS edition.

### editorial_review_json

Forensic editorial diagnosis of the first draft or latest full redraft.

### draft_final_markdown

Final automated revision delivered to the human editor.

### human_notes

Human-owned freeform notes.

Automations may read them when relevant and must never overwrite them without explicit human instruction.

## 7. JSON storage

Structured objects are stored as compact valid JSON strings.

Use `null` for unknown scalar values.

Use `[]` for empty arrays.

Do not store Markdown code fences around JSON.

Stringify exactly once before Sheet write and parse exactly once after read.

## 8. Markdown storage

Draft fields store plain Markdown text.

Do not wrap Markdown in JSON when writing to the draft columns.

## 9. Canon authority

After `canon_bible_json` is written, downstream agents should treat it as the current truth of the fictional production.

The Casting Director, Edition Architect, and Edition Writer may not casually change canon because a different choice occurs to them.

If a foundational issue is discovered, reopen canon explicitly.

## 10. Revision routes

`editorial_review_json.revision_route` determines the next automated action:

- `NONE` -> deterministic QA and human review
- `PROSE` -> Revision Writer
- `EDITION` -> Edition Architect, Edition Writer, then Forensic Editor again
- `CANON` -> Canon Builder rescue, then recast/replan/redraft/review

Limit automated rescue to one reentry cycle in v1.

After one rescue cycle, deliver the best available version to the human editor with unresolved issues clearly recorded.

## 11. Overwrite rules

- preserve identity fields forever
- preserve `human_notes`
- never erase a nonblank field merely because a downstream stage is skipped
- when a controlled rescue stage regenerates canon or downstream artifacts, overwrite only the fields that logically depend on the changed artifact
- do not regenerate creative work merely because a Sheet write failed

## 12. Human authority

The final automated status is `READY_FOR_HUMAN_REVIEW`, not `PUBLISHED`.

The human editor decides whether the edition publishes, receives another manual revision, returns to development, or is abandoned.
