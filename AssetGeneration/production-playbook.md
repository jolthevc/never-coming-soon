# Three-slide production playbook

## The product and the role

Never Coming Soon is a social-first entertainment brand. The artifact is the sensation of discovering a real-feeling movie or show that does not exist. Long-form material is source material; the carousel must be understandable at feed speed and rewarding after a swipe. The image producer owns design and visual execution. The packet owns editorial content.

The default arc is **tell me the idea → tell me the movie → show me the artifact**. Do not carry forward the prior six-slide character/texture/CTA layout or the still older poster/premise/brand-close layout as the default.

## Intake and text authority

- Locate the single matching row in the Ideas sheet when given a title. Read `ig_packet_json` in full and check its version and slide definitions. Title matches that are ambiguous require disambiguation.
- Extract the exact title, format, hook, plot paragraphs, poster tagline, rating, format descriptor, footer/billing copy, art direction, image prompt, and any explicit slide/page instructions. Keep a copy of the source text alongside work files for comparison.
- Preserve packet copy **verbatim**, including capitalization, punctuation, accents, and paragraph boundaries. Never silently shorten, summarize, polish, invent, or substitute copy. Place exact text with deterministic typography when image generation cannot reproduce it reliably.
- The current user's explicit changes outrank an older packet. If a required current-format field is absent, flag it rather than silently treating a legacy `slide_1.cover_poster` as the hook or `slide_3.ncs_close` as the poster. For an explicitly exploratory sample, label any provisional copy clearly.
- The caption is separate. It may ask for casting, viewing interest, or another story-native response. A dedicated engagement or traffic slide is retired. No default “LINK IN BIO.”

## Shared technical contract

- Exactly three independently usable PNGs at **1080 × 1440 px**, all in the same orientation and numbered `01-...`, `02-...`, `03-...`.
- Work inside generous safe margins for title, body, rating, and billing. Inspect at full size and at phone-feed size.
- Slides 1 and 2 share a warm cream editorial surface, deep navy text, a restrained red accent, consistent left alignment and margins, and disciplined type hierarchy. Exact colors, fonts, and coordinates should be measured from an approved master, not guessed afresh each time.
- Slide 3 belongs primarily to the fictional production. Its palette, medium, type, and central image should change with the film. Recurring NCS identity can appear as restrained presentation credit, not as an oversized pasted-on logo.
- A quiet NCS signature/page count on the first two slides is optional unless prescribed by the packet. If used, place both at the **same fixed baseline** and coordinates across those slides. Let the poster stand alone without a social page counter by default; obey an explicit packet instruction otherwise. Do not let a footer drift or overlap imagery.

## 01: Hook

Purpose: a stranger understands in a second that this is a movie/show idea and what makes it watchable.

Text-first, no photo by default. Hierarchy: huge **MOVIE IDEA** or **SHOW IDEA**, prominent title, then one strong one-sentence hook. In the approved Shared Leash direction the category is the first and largest object, the title is red, and the hook is a substantial readable navy line beneath it. Cream background, spare composition, minimal branding. No fake poster credits, rating, dense labels, glossy key art, or decorative motif that competes with the idea. Keep the hook exact from the packet, even when line wrapping changes.

## 02: The Plot

This is the main editorial payload, rather than a transitional premise card. Start with a small title kicker, subtle red rule/accent, and a clear **THE PLOT** heading. Set the packet's exact plot in one highly readable navy column, normally two or three distinct paragraphs. The expected range is roughly 110–170 words, but the actual packet wins. Use line length, leading, vertical spacing, and a sensible margin to make reading pleasant on a phone. Keep the background one continuous warm cream color so a tint boundary cannot cross the copy.

No large cinematic image by default. A tiny movie-specific accent is acceptable only when it helps and never at the expense of body size. Do not squeeze the text to make space for artwork. If a full-length packet genuinely cannot fit legibly at 1080 × 1440, report the conflict for an editorial decision; do not edit the text or add a surprise extra slide.

## 03: Poster

The payoff is a poster somebody could save or share on its own. Develop a singular campaign image and choose whatever medium makes this movie most compelling: graphic, photographic, illustrated, painted, typographic, object-led, or character-led. See [poster direction](poster-direction.md) before starting. Follow exact packet title, tagline, rating, descriptor and footer; do not invent real actors or production credits. The poster is allowed and encouraged to feel visually different from the cream editorial slides.

## Production workflow

1. **Read** the packet, this guide, and the approved example limitations. Identify exact fields and conflicts before designing.
2. **Art direct** the poster first conceptually: sketch multiple distinct hero constructions and select the one that communicates the concept and genre with the fewest elements. Ensure Slides 1 and 2 remain the editorial system.
3. **Generate** key art or textures where needed. Reserve clean negative space for exact text. AI image generation is suited to imagery, not reliable spelling, complex footer typography, or surgery on an approved image.
4. **Compose** type and repeatable editorial elements deterministically, including packet copy, rating, billing, and optional footers. Preserve source art when revising. Keep a layered or reproducible working source if practical.
5. **Check** every line against the packet; image dimensions; paragraph breaks; number/order; text contrast; small-screen legibility; and visual continuity. Inspect each PNG individually, not only a montage.
6. **Deliver** three numbered image files and a concise note about any unresolved packet conflict. A contact sheet may supplement review.

## Revision discipline

- Identify which slide and region the user actually wants changed. Keep approved imagery and copy untouched elsewhere.
- Regenerating a whole approved slide to adjust text often changes faces, objects, colors, and alignment. Recompose or edit the local layer where possible.
- Reinspect the final output for ghosts from removal/inpainting, clipped headings, drifting footer baselines, wrong page counts, and text collision.
- Change the shared system only after the user accepts the new direction. A single movie's palette or motif is not a universal brand rule.

## Review questions

- Can a new viewer read “MOVIE IDEA,” the title, and the hook immediately?
- Can the exact full plot be read comfortably in two or three clear paragraph blocks without a blob of copy?
- Does the poster leave one memorable visual idea, communicate the genre, and look like an intentionally designed campaign artifact?
- Do the three slides make the same fictional production feel real while preserving the NCS editorial signature on the first two?
