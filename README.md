# Never Coming Soon

Never Coming Soon is an imaginary entertainment studio and media publication for the best movies and shows that do not exist.

This repository contains the durable creative governance, prompt contracts, structured schemas, workflow specifications, and approved writing-calibration examples used by the Never Coming Soon system.

## System architecture

Never Coming Soon uses distinct creative workflows.

1. **Ideation** finds, remembers, compares, curates, lightly expands, and Development-Selects fertile creative concepts.
2. **Generation** develops a selected concept into a real internal movie or show, optionally grounds it with research, challenges it, freezes canon, casts it, architects the public edition, drafts it, forensically reviews it, revises it, and delivers it for human review.
3. **Visual production** will be designed separately after text canon and the final edition are stable.

The workflows are intentionally separate.

Ideation explains why an idea deserves development. Generation owns the actual storytelling and may materially improve the title, format, characters, relationships, setting, plot, scenes, ending, casting, and other provisional choices.

The handoff object from Ideation to Generation is the **Development Packet**.

The core Generation principle is:

**Develop the production before writing the article.**

## Source of truth

- `Governance/` contains durable creative standards and data contracts.
- `Prompts/Ideation/` contains the six Ideation agent prompt pairs.
- `Prompts/Generation/` contains the Generation agent prompt pairs.
- `Schemas/` contains machine-readable structured-output contracts.
- `WorkflowSpecs/ideation-workflow-v1.md` contains the n8n Ideation specification.
- `WorkflowSpecs/generation-workflow-v1.md` contains the n8n Generation specification.
- `Examples/Gold/` contains explicitly approved writing-calibration examples.

Google Sheets is the persistent working memory and state layer.

GitHub is the source of truth for governance, prompts, schemas, workflow specifications, and approved calibration examples.

n8n orchestrates the system.

## Ideation state

The Ideation workflow uses one `Ideas` tab and one row per concept.

Its terminal creative object is `development_packet_json`.

## Generation state

Generation uses a separate `Productions` workspace governed by `Governance/ncs-generation-data-contract.md`.

One row equals one developed production.

Generation ends at `READY_FOR_HUMAN_REVIEW` rather than publishing automatically.

The human editor remains the final creative authority.

## Writing calibration

The house voice is governed by `Governance/ncs-voice-constitution.md`.

The first approved gold-standard film example is:

`Examples/Gold/film-01-the-tell.md`

Gold examples teach voice, craft, section behavior, and editorial judgment. They are never story templates.

The Tell primarily calibrates character-driven sports comedy-drama, character-first presentation, observed-performance casting voice, proof of existence, evidence of spectatorship, selective scene treatment, and protected response under pressure.

One example is not a universal genre template. Future gold examples should deliberately broaden the calibration set.

## Current implementation status

- Ideation package: specified and being implemented in n8n.
- Generation package: governance, prompts, schemas, and workflow specification drafted for implementation.
- Gold-standard reference film example 01: locked and added.
- Film editorial doctrine: calibrated on The Tell; genre stress testing remains an active next step.
- Television editorial architecture: provisional pending dedicated calibration.
- Visual workflow: intentionally deferred.
