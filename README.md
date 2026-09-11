# Never Coming Soon

Never Coming Soon is an imaginary entertainment studio and media publication for the best movies and shows that do not exist.

This repository contains the durable creative governance, prompt contracts, structured schemas, and workflow specifications used by the Never Coming Soon automation system.

## System architecture

Never Coming Soon uses two distinct automation workflows.

1. **Ideation workflow**: generates, remembers, compares, curates, lightly expands, and development-selects fertile creative concepts.
2. **Generation workflow**: develops selected concepts into full productions, drafts editions, reviews quality, revises, and reaches a publish decision.

The workflows are intentionally separate.

Ideation creates a strong seed and explains why it is worth developing. Generation owns the actual storytelling and may materially improve the title, format, characters, relationships, setting, plot, scenes, ending, casting, and other provisional choices.

The handoff object between the workflows is the **Development Packet**.

## Source of truth

- `Governance/` contains durable standards and constitutions.
- `Prompts/Ideation/` contains the six Ideation agent system and user prompt pairs.
- `Schemas/` contains the six machine-readable Ideation output contracts.
- `WorkflowSpecs/ideation-workflow-v1.md` contains the n8n Ideation implementation specification.

Google Sheets is the working memory and state layer for ideas.

GitHub is the source of truth for governance, prompts, schemas, and workflow specifications.

n8n orchestrates the system.

## Current implementation status

The Ideation package is specified for implementation.

The Generation operating principles exist in governance, but its node-level workflow, prompts, and schemas will be designed separately after the Ideation workflow is stable.
