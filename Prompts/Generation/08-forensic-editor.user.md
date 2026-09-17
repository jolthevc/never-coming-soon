CANON BIBLE
{{canon_bible_json}}

EDITION PLAN
{{edition_plan_json}}

CURRENT DRAFT
{{current_draft_markdown}}

DETERMINISTIC DIAGNOSTICS
{{pre_review_diagnostics_json}}

REVIEW PHASE
{{review_phase}}

Review this exact draft rigorously but fairly. Diagnose rather than rewrite.

Use the diagnostics as leads, not automatic failure criteria. Confirm any warning against the actual draft before treating it as a problem.

Before listing weaknesses or revision requirements, identify the strongest 2 to 4 things in this exact production / edition that should not be damaged. Put those concrete strengths in `strengths_to_preserve`. Prefer specific sources of value such as the native pleasure source, a relationship, a scene or image, a comic / genre mechanism, a tonal quality, or an especially effective structural choice. Do not fill the list with generic praise, and do not manufacture four if only two are truly important.

When a proposed fix could endanger one of those strengths, use the `preserve` field in the relevant `revision_requirements` item to state what must survive the fix.

Preserve what works. Do not recommend revision merely because the diagnostics contain warnings or because a theoretically more optimized version can be imagined.

Assign `overall_score` to this exact draft. If `review_phase` is FINAL, remember that this score may be persisted to the delivered Google Doc and Ideas row, so it must correspond to the exact text in CURRENT DRAFT.

Return only valid JSON matching the editorial-review schema.
