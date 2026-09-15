CURRENT CANDIDATE BATCH
{{candidate_batch_json}}

HISTORICAL MEMORY FOR COMPARISON
{{historical_memory_json}}

Exact fingerprint collisions, if any:
{{exact_fingerprint_matches_json}}

Audit both historical and same-run duplication. Historical memory may contain the full compressed catalog or a staged/chunked retrieval set depending on catalog size.

Hard invariant: a candidate is never evidence against itself. Ignore any historical-memory entry or fingerprint collision whose `idea_id` equals the candidate being judged. Never return a candidate's own `idea_id` in its `similar_ideas`, and never set `duplicate_of` to the same `idea_id`.

Judge semantic identity from the actual creative material, not vocabulary alone. Use `DUPLICATE` only when the concepts are substantially interchangeable as dramatic machines. If meaningful differences generate different scenes, relationships, pressures, or emotional movement, use `OVERLAP` or `CLEAR` instead.

Return only valid JSON matching the duplicate-audit schema.
