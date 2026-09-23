# Never Coming Soon
## Social Asset Standard v3.0

## 1. Purpose

This standard governs the canonical social package for a Never Coming Soon production.

Never Coming Soon is social-first.

The core product is the movie or show idea itself, especially the plot.

The social package should make a stranger understand the idea immediately, give them enough story to want the production, then reward them with a poster.

Core doctrine:

**Idea -> plot -> poster.**

The intended reaction is:

**I would watch this. Why does this not exist?**

## 2. Source of truth

Every completed production receives one schema-valid `ig_packet_json`.

The packet is built from frozen final canon plus any explicit human campaign direction.

A long-form article is not required input.

The packet is the exact editorial handoff for manual asset generation. Public copy should be used verbatim.

## 3. Canonical canvas and footer

Default carousel canvas:

- 1080 x 1440 pixels
- 3:4 portrait
- identical dimensions for all three slides
- generous safe margins

Every slide includes a quiet footer:

- bottom-left: `Never Coming Soon`
- bottom-right: page number in `0X / 03` format

Keep the footer subtle.

## 4. Locked three-slide spine

Every canonical carousel contains exactly three slides:

1. Hook
2. Plot
3. Poster

Do not add a mandatory character slide, cinematic texture slide, or branded close.

The carousel should be short enough that the idea itself remains the product.

## 5. Slide 1: Hook

Slide 1 is brutally clear.

Its job is to stop the scroll and make the viewer understand what the post is before they need to interpret artwork.

Required:

- `MOVIE IDEA` for FILM
- `SHOW IDEA` for SERIES or LIMITED_SERIES
- final title
- one-sentence hook

Default treatment:

- text-first
- Warm Ivory or similarly quiet editorial surface
- deep navy typography
- one restrained accent color
- no cinematic image
- no poster treatment
- no billing block
- no visual puzzle

Slide 1 should feel like an exceptionally well-designed social text card.

The hook must explain the central situation quickly.

## 6. Slide 2: The Plot

Slide 2 is the main reading experience.

This is where the audience gets the thing they came for.

It should usually be almost entirely editorial and text-led rather than visually elaborate.

Required:

- small production-title kicker
- `THE PLOT`
- exact body copy
- layout direction
- NCS footer and page number

### Copy

Normally use roughly 110 to 170 words when the story supports it.

Prefer 2 to 3 short paragraphs.

The copy should:

- explain the setup clearly
- introduce the central people naturally through the story
- show the pressure, engine, or complication
- give enough specificity that the movie begins playing in the reader's head
- stop before resolving the story or revealing protected payoff

Core rule:

**Give away the setup generously. Withhold the payoff, not the premise.**

This should not read like:

- a film-school synopsis
- marketing copy
- a theme statement
- a character dossier
- a teaser built from vagueness

It should read like someone telling you a movie idea that gets better the more they explain it.

### Layout

Slide 2 should be designed for reading on a phone.

Default:

- warm cream editorial background
- no large bespoke image
- no 50/50 image split
- no cinematic still required
- strong hierarchy
- comfortable body-text size
- generous line spacing
- 2 to 3 visually distinct paragraph blocks
- enough negative space that the copy does not feel cramped

A very small project-specific accent, line, icon, texture, or understated visual detail may be used when it improves the page, but it should never compete with the plot.

The plot copy is the hero.

## 7. Slide 3: Poster

Slide 3 is the payoff.

It should feel like the poster for the movie or show the audience now understands and wants.

Required packet fields include:

- title
- tagline
- plausible rating
- concise format descriptor
- restrained footer copy
- art direction
- image prompt

The poster may be:

- photographic
- graphic
- illustrated
- painted
- typographic
- minimal
- surreal
- object-led
- character-led

Choose the strongest concept for the production.

Do not imply real actors, filmmakers, studios, crews, festivals, or location authorities participated.

Do not use real actor likenesses by default.

The poster should feel like the reward for reading the plot.

## 8. What NCS owns

Across releases, NCS should consistently own:

- Slide 1 architecture
- Slide 2 editorial reading system
- core typography
- generous margin discipline
- Warm Ivory and deep navy as default editorial surfaces
- small red or production-specific accent
- quiet footer and pagination
- exact-copy discipline

## 9. What the production owns

The fictional production mainly owns the poster:

- poster palette
- title treatment
- central visual
- production-specific motif
- campaign concept

Do not force every idea to support a large bespoke visual system before the poster.

## 10. Manual asset-generation boundary

Asset generation is performed manually outside n8n.

The asset-generation model should not decide:

- what the hook is
- what the plot says
- how much story to reveal
- what the tagline is
- what order the slides use
- what the rating or format descriptor says

Those decisions belong in `ig_packet_json`.

The art model may exercise creativity in executing Slide 3 poster art and in polishing the editorial layout of Slides 1 and 2.

Do not rewrite packet copy.

## 11. Caption

The packet includes one exact launch caption.

Default:

- normally 55 to 90 words
- usually two short paragraphs
- concrete and conversational
- no hashtags by default
- no generic praise
- no backstage process language

The caption should complement the carousel rather than merely repeat Slide 2.

Engagement prompts such as casting discussion or `Would you watch it?` belong naturally in the caption when useful rather than requiring a dedicated CTA slide.

## 12. Integrity

All public copy must:

- match final canon
- contain no em dash character
- contain no internal workflow terminology
- contain no backstage technology language
- avoid false real-world participation claims
- avoid explicit spoiler-management language

## 13. Packet quality test

Before persistence, verify:

- exactly three slides
- Slide 1 clearly says MOVIE IDEA or SHOW IDEA
- Slide 1 is understandable in under a second
- Slide 2 is primarily text-led
- Slide 2 plot copy is complete enough to satisfy curiosity but does not reveal the protected payoff
- Slide 2 is split into readable paragraphs with mobile-friendly spacing
- Slide 2 does not depend on a bespoke cinematic image
- Slide 3 works as a genuine poster payoff
- every slide has the NCS footer and correct page number
- all exact public copy is final and execution-ready
- the final object matches `Schemas/ig-asset-packet.schema.json`

If not, repair the packet before persistence.
