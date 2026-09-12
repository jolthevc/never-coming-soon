# Never Coming Soon
## Ideation Data Contract Dashboard Amendment v1.0

This document amends `Governance/ncs-data-contract.md` for dashboard-driven Concept Intake.

No new Sheet columns are added.

The `ideation_mode` field gains one additional allowed provenance value:

- `concept_intake`

This value means the concept entered through the human-directed CONCEPT_INTAKE path rather than one of the ten generative starting modes.

All existing ten creative mode values remain valid.

If the live Google Sheet uses dropdown validation for `ideation_mode`, add `concept_intake` to that validation list.

All other ownership, persistence, JSON, overwrite, and minimalism rules remain unchanged.