# Never Coming Soon
## Legacy Generation Workflow Notice

This filename is retained for compatibility only.

The separate Generation workflow has been retired from the normal Never Coming Soon architecture.

The canonical n8n implementation is now one unified creative workflow documented in:

`WorkflowSpecs/ideation-workflow-v1.md`

That workflow continues from Ideation through:

`Development Select -> Production Builder -> Production Treatment -> Social Release Builder -> DRAFTED`

Do not implement or maintain a second normal n8n Generation workflow.

## Canonical downstream behavior

For each newly selected concept, the unified workflow runs only two additional creative calls:

1. Production Builder
2. Social Release Builder

Optional research may occur when explicitly required.

The normal path does not require:

- Casting Director
- Edition Architect
- Edition Writer
- Forensic Editor
- Revision Writer
- editorial scoring
- long-form article generation
- automatic asset generation

## Asset boundary

The unified n8n workflow ends after:

- final Canon Bible validation
- deterministic Production Treatment persistence
- schema-valid `ncs_ig_v3` persistence
- Ideas row update to `DRAFTED`

Visual asset generation is manual and intentionally outside n8n.

## Long-form boundary

Legacy long-form prompts and governance may remain in the repository for optional later editorial work.

They are not part of the normal production path.

For current workflow behavior, always defer to:

- `WorkflowSpecs/ideation-workflow-v1.md`
- `Governance/ncs-data-contract.md`
- `Governance/ncs-generation-data-contract.md`
- `Governance/ncs-social-asset-standard.md`
- `Schemas/ig-asset-packet.schema.json`
