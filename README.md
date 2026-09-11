# Never Coming Soon

Never Coming Soon is an imaginary entertainment studio and media publication for the best movies and shows that do not exist.

This repository contains the durable creative governance, prompt contracts, structured schemas, and workflow specifications used by the Never Coming Soon automation system.

## System architecture

Never Coming Soon uses two distinct automation workflows.

1. **Ideation workflow**: generates, remembers, compares, curates, and development-selects fertile creative concepts.
2. **Generation workflow**: develops selected concepts into full productions, drafts editions, reviews quality, revises, and reaches a publish decision.

The workflows are intentionally separate. Ideation does not write the final production. Generation is allowed to materially improve titles, characters, plot, setting, ending, format, casting, and other developmental elements.

The handoff object between the two workflows is the **Development Packet**.

## Source of truth

- `Governance/` contains durable standards and constitutions.
- `Prompts/` contains node-level system and user prompts.
- `Schemas/` contains machine-readable output contracts.
- `WorkflowSpecs/` contains the n8n implementation specification.

Google Sheets is the working memory and state layer for ideas. GitHub is the source of truth for governance and prompts. n8n orchestrates the system.
