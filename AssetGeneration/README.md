# NCS asset generation: start here

This folder is the working handoff for a new Never Coming Soon image-production chat. It records the social-first, three-slide system agreed in September 2026. The goal is to make each fictional project instantly understandable and desirable, then reward the reader with a credible movie or show poster.

**Default carousel:** `01 Hook → 02 The Plot → 03 Poster`. Export three **separate 1080 × 1440 PNGs** (3:4 portrait) in posting order. A review contact sheet is optional; it never replaces the separate files. The caption carries participation. There is no default CTA or link-in-bio slide.

Read in this order:

1. [Production playbook](production-playbook.md): intake, copy authority, layouts, production, QA, and revisions.
2. [Poster direction](poster-direction.md): the iconic image standard and convincing poster furniture.
3. [Shared Leash reference](examples/shared-leash/README.md): images and their exact approval limits.

## Source precedence

1. The current user's specific instructions and corrections.
2. The matching row's **`ig_packet_json`**, when available, for editorial facts, exact text, paragraph breaks, rating, format, footer, and prescribed placement.
3. Approved assets for visual judgment, limited to what was actually approved.
4. This asset-generation guide for defaults and craft decisions.

The older `Governance/ncs-social-asset-standard.md`, `Governance/ncs-visual-constitution.md`, `Schemas/ig-asset-packet.schema.json`, and Generation packet-builder prompt document the previous **poster / premise / brand-close** pipeline. They are not instructions for the current manual three-slide asset workflow. The upstream packet generator has not yet been migrated; a legacy packet may lack the new hook and plot fields or include retired close-slide copy. Surface a material conflict rather than inventing new canonical text. Do not change the packet or its narrative content just to fit a design.

## Starting a new chat

Ask the assistant to read this folder before generating. Give the movie/show title or paste `ig_packet_json`. With title-only input, find exactly one matching title in the **Never Coming Soon — Ideas** Google Sheet, read that row's `ig_packet_json`, and work from it. If the column is blank or the packet belongs to the older format, flag what is missing. The sheet's other columns can help locate the row, but cannot override the packet.

NCS positioning: **the best movies and tv shows that don’t exist.** The carousel should make someone think “I would watch this,” not “I have read a pitch.”
