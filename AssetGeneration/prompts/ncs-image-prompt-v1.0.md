# NCS Image-Generation Prompt

version: ncs-image-prompt-v1.0
status: active
updated: 2026-10-10
used by: n8n workflow "NCS - Poster Art" (OpenAI Images Edits API)

This file is the locked house style for every NCS poster artwork call. n8n loads it by this exact versioned path and fills the {{placeholders}}. To change the style, commit a new file (ncs-image-prompt-v1.1.md, v2.0, ...) and point the workflow at it. Never edit a released version in place.

## Art-director guidance (read by the concept model)

- Produce three campaign directions per film by default. Each must be a different idea about the film, not a different crop, palette, or random seed of the same picture. Vary the central metaphor, the point of entry into the story, and the composition.
- Each direction owns its palette and composition, derived from this production's world. The house style below governs finish and craft only, never colour scheme or layout.
- One dominant, story-specific idea per poster. Object-led or graphic constructions beat literal scene illustration. No identifiable faces; any figures are partial, silhouetted, or turned away.
- Never borrow another film's symbolic objects from the reference posters.
- The artwork carries no type. Title, credits, rating, and billing are composited later by the console from ig_packet_json.

<!-- IMAGE_PROMPT_START -->
NEVER COMING SOON house poster artwork (prompt ncs-image-prompt-v1.0).

STYLE LOCK, applies to every NCS poster:
Illustrated, vintage-print-inspired theatrical poster artwork with a tactile, premium finish. Think hand-pulled screen print or stone lithograph: visible paper grain, slight ink spread, subtle registration offset, flat or gently graded colour fields, painterly or graphic rendering of objects. An intentional, designed composition with one dominant focal idea and generous negative space. Restrained, sophisticated colour. It must feel like a collectible printed one-sheet, not a photograph, not glossy 3D, not generic AI key art.

REFERENCE IMAGES:
{{reference_note}}
Use the reference images only for craft: print texture, paper quality, ink behaviour, level of finish, and design restraint. Do not copy their subjects, symbolic objects, layout, palette, or any lettering they contain.

THIS PRODUCTION:
Genre: {{genre}}
Direction: {{concept_name}}
Image: {{image_prompt}}
Palette, owned by this production: {{palette}}
Composition: {{composition}}

FORMAT:
Portrait 2:3 artwork. Keep the lower fifth relatively calm so the console can set the title there. An ivory paper border is allowed when it suits the design.

NO TYPOGRAPHY:
Render artwork only. No title, no credits, no billing block, no rating, no tagline, no letters, numbers, words, signage text, logos, watermarks, or signatures anywhere in the image, including on objects, packaging, screens, or signs. Where a surface would normally carry text, leave it blank or abstract.

AVOID:
Photorealism, glossy CGI, lens flare, cast collages, multi-scene montage, recognisable or celebrity faces, real studio marks, arbitrary prop piles.
<!-- IMAGE_PROMPT_END -->
