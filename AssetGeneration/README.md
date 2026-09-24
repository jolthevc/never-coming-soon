# NCS asset generation: start here

This folder is the working handoff for a new Never Coming Soon image-production chat. It records the social-first, three-slide system agreed in September 2026. The goal is to make each fictional project instantly understandable and desirable, then reward the reader with a credible movie or show poster.

**Default carousel:** `01 Hook → 02 The Plot → 03 Poster`. Export three **separate 1080 × 1440 PNGs** (3:4 portrait) in posting order. A review contact sheet is optional; it never replaces the separate files. The caption carries participation. There is no default CTA or link-in-bio slide.

Read in this order:

1. [Production playbook](production-playbook.md): intake, copy authority, layouts, production, QA, and revisions.
2. [Poster direction](poster-direction.md): the iconic image standard and convincing poster furniture.
3. [Shared Leash reference](examples/shared-leash/README.md): images and their exact approval limits.

## Source precedence

1. The current user's specific instructions and corrections.
2. The matching row's **`ig_packet_json`** for editorial facts, exact text, paragraph breaks, poster copy, footer, and prescribed placement.
3. Current NCS governance and schema files in the repository.
4. Approved assets for visual judgment, limited to what was actually approved.
5. This asset-generation guide for defaults and craft decisions.

The canonical upstream packet version is now **`ncs_ig_v3`** and uses the same three-slide structure as this manual workflow:

1. Hook
2. The Plot
3. Poster

The current upstream source of truth is:

- `Governance/ncs-social-asset-standard.md`
- `Governance/ncs-visual-constitution.md`
- `Schemas/ig-asset-packet.schema.json`
- `Prompts/Generation/10-ig-asset-packet-builder.system.md`

Do not translate older packets into v3 yourself. If a Sheet row has a blank packet or a legacy packet such as `ncs_ig_v1` or `ncs_ig_v2`, flag it for regeneration rather than inventing missing hook or plot copy.

## Starting a new chat

Ask the assistant to read this folder before generating. Give the movie/show title or paste `ig_packet_json`.

With title-only input:

1. find exactly one matching title in the **Never Coming Soon — Ideas** Google Sheet
2. read that row's `ig_packet_json`
3. require `version = ncs_ig_v3`
4. execute the packet verbatim

If the packet is blank, malformed, or legacy, stop and flag the mismatch. The Sheet's other columns can help locate the row but cannot replace or override the packet.

NCS positioning: **the best movies and tv shows that don’t exist.** The carousel should make someone think “I would watch this,” not “I have read a pitch.”
