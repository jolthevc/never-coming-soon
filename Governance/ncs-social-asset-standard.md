# Never Coming Soon
## Social Asset Standard v2.0

## 1. Purpose

This standard governs the canonical social package for a Never Coming Soon production.

Never Coming Soon is social-first.

The social package is not an advertisement for a long-form article. It is a primary way the audience experiences the fictional movie or show.

Core doctrine:

**Make the audience understand the idea, meet the people, feel the movie, then reward them with the poster.**

The intended reaction is:

**I would watch this. Why does this not exist?**

## 2. Source of truth

Every completed production receives one schema-valid `ig_packet_json`.

The packet is built from frozen final canon and any explicit human campaign direction.

A long-form article is not required input.

The packet is exact execution guidance for downstream asset generation. Public copy in the packet must be placed verbatim unless a field explicitly permits downstream judgment.

## 3. Canvas

Default carousel canvas:

- 1080 x 1440 pixels
- 3:4 portrait
- identical dimensions for all six slides
- generous safe margins for typography and logos

Image generation creates visual material. Exact text, logos, lines, and layout should be composited deterministically whenever practical.

## 4. Locked six-slide spine

Every canonical carousel contains exactly six slides:

1. Hook
2. Premise
3. Characters
4. The Movie
5. Poster
6. NCS Close

The architecture is fixed. The movie-specific art direction is not.

## 5. Slide 1: Hook

Slide 1 is brutally clear.

Its job is to stop the scroll and make the viewer understand what the post is before they need to interpret artwork.

Required:
- `MOVIE IDEA` for FILM
- `SHOW IDEA` for SERIES or LIMITED_SERIES
- final title
- one-sentence hook
- small `Never Coming Soon` signature

Default treatment:
- text-first
- warm cream or similarly quiet editorial surface
- deep navy typography
- one restrained accent color
- no poster treatment
- no billing block
- no rating
- no cinematic key art requirement
- no visual puzzle

Slide 1 should feel closer to an exceptionally well-designed social text card than a movie poster.

The hook must explain the central situation quickly. It should not tease through vagueness.

## 6. Slide 2: Premise

Slide 2 establishes the world and setup.

Default treatment:
- cinematic image plus editorial text
- two short paragraphs when the copy has two natural movements
- NCS typography, spacing, and editorial framing around movie-specific imagery
- optional small section label when it helps navigation

The image should feel like a frame from the movie or show, not promotional key art.

The copy should explain the situation, central conflict, and engine clearly enough that the audience fully understands the concept.

Preserve the existing NCS premise-copy discipline:

**Give away the setup generously. Withhold the experience, not the premise.**

Avoid:
- vague teaser language
- theme summary
- plot synopsis
- one dense block of text
- generic claims that the movie is heartfelt, cinematic, funny, gripping, or compelling

## 7. Slide 3: Featured Characters

Slide 3 creates human attachment.

Featured-character rule:
- two characters by default
- three for genuinely ensemble-driven concepts
- four only when necessary
- never inventory the full cast

Choose the people whose presence most increases desire for the production.

Copy for each featured character:
- name
- one concise description
- behavior, contrast, desire, contradiction, or chemistry
- no résumé-style dossier
- no full arc summary

### Character visual treatment

The default NCS character treatment is an **editorial character study** or **NCS casting-room sketch**.

Use:
- expressive graphite or ink linework
- selective flat color or loose watercolor wash
- visible paper texture
- specific wardrobe, posture, props, and expressions
- slightly imperfect authored marks
- two or three colors drawn from the production's visual world
- one relational composition rather than isolated portrait boxes whenever possible

Do not use:
- real actor likenesses
- faux-photoreal celebrity casting
- glossy skin
- police-composite or mugshot language
- generic floating headshots

The characters should be recognizable enough for continuity but open enough that the audience can still imagine casting.

## 8. Slide 4: The Movie

Slide 4 sells the experience of watching the production.

It should be the most immersive slide.

Default treatment:
- image-dominant
- roughly 70 to 80 percent visual when composition supports it
- one compact paragraph or a few tightly connected details
- restrained NCS editorial framing

The material may draw from:
- recurring situations
- world texture
- memorable locations
- social dynamics
- genre pleasure
- comedy, tension, romance, fear, or spectacle
- signature scenes
- emotional current
- proof-of-existence details

This is not a plot summary.

The test is:

**Can I already see scenes from this?**

Texture must still increase desire. Do not include atmosphere merely because it sounds evocative.

## 9. Slide 5: Poster

Slide 5 is the payoff.

It should feel like the poster for the movie or show the audience now understands and wants.

The poster belongs primarily to the fictional production.

Required packet fields include:
- title
- tagline
- plausible rating
- concise format descriptor
- restrained footer copy
- art direction
- image prompt

The central visual, title treatment, palette, and poster concept may vary completely by production.

Do not imply that real actors, filmmakers, studios, crews, festivals, or location authorities participated.

Do not use real actor likenesses by default.

The poster should feel like a reward for swiping, not another explanation slide.

## 10. Slide 6: NCS Close

Slide 6 returns to Never Coming Soon.

Priority order:
1. participation question
2. NCS identity
3. locked slogan
4. optional secondary action

Locked slogan:

`the best movies and tv shows that don't exist.`

The participation question should be native to the production.

Good examples:
- Who are you casting?
- Would you watch it?
- Movie or series?
- Who plays June?
- Which poster would you pick?

Avoid generic engagement bait when a more specific question exists.

A link or long-form action may appear as a secondary action when one genuinely exists. It is not the primary purpose of the close.

The visual should conclude the production's world with a simplified motif, silhouette, environment, or object rather than becoming a generic corporate card.

## 11. Visual rhythm

The intended progression is:

**explain it -> establish it -> humanize it -> immerse me -> make it real -> invite me in**

A useful density rhythm is:

1. typography
2. image plus substantial copy
3. illustration plus compact character copy
4. large image plus minimal copy
5. full poster
6. graphic brand close

## 12. What NCS owns

Across releases, NCS should consistently own:
- Slide 1 architecture
- core editorial typography
- generous margin discipline
- warm cream and deep navy as default editorial surfaces
- small uppercase navigation labels when used
- body-copy treatment
- Slide 3 illustrated character-study language
- Slide 6 brand structure
- exact-copy discipline

## 13. What the production owns

Each fictional production may own:
- cinematic imagery
- accent color
- title typeface
- image grading
- wardrobe and production design
- recurring motifs
- poster concept
- closing-card imagery
- participation question

The goal is one recognizable publication releasing radically different movies, not one rigid template wearing different costumes.

## 14. Visual continuity

Slides 2, 4, and 5 must feel like they belong to the same production.

The packet must provide:
- world style
- palette
- cinematography
- character visual descriptions
- wardrobe language
- posture and energy
- identifying details

When the same character appears across generated assets, preserve identity and wardrobe logic unless the packet explicitly calls for a change.

Slide 3 is intentionally illustrated rather than cinematic, but the character details and production palette should still match the same fictional people and world.

## 15. Image-model boundary

The image model is an executor, not an editor.

It should not decide:
- what the slide is about
- which characters matter
- what copy to write
- what order the carousel uses
- what the poster tagline is
- what the CTA is
- how many characters appear
- what story information to reveal

Those decisions belong in `ig_packet_json`.

The image model may exercise visual creativity inside the supplied composition, style, lighting, wardrobe, environment, and mood direction.

All exact text should be composited outside the image model whenever practical.

## 16. Caption

The packet includes one exact launch caption.

Keep the existing voice principle:

**Give away the setup generously. Withhold the experience, not the premise.**

Default:
- two short paragraphs
- concrete and conversational
- plot and tension first
- engine or recurring pressure second
- no hashtags by default
- no generic praise
- no fake participation claims
- no backstage process language

The caption may be used for Instagram or adapted downstream for X.

## 17. Integrity

All public copy must:
- match final canon
- contain no em dash character
- contain no internal workflow terminology
- contain no backstage technology language
- avoid false real-world participation claims
- avoid explicit spoiler-management language

## 18. Packet quality test

Before persistence, verify:
- Slide 1 is understandable in under a second
- Slide 1 clearly says MOVIE IDEA or SHOW IDEA
- Slide 2 gives the setup generously
- Slide 2 uses readable paragraphing
- Slide 3 features two characters by default and never more than four
- Slide 3 copy creates attachment rather than inventory
- Slide 3 uses the editorial character-study visual language
- Slide 4 makes the viewing experience vivid without summarizing the plot
- Slide 5 works as a real poster payoff
- Slide 6 prioritizes participation and brand memory
- Slides 2, 4, and 5 share a coherent fictional world
- all exact public copy is final and execution-ready
- the final object matches `Schemas/ig-asset-packet.schema.json`

If not, repair the packet before persistence.
