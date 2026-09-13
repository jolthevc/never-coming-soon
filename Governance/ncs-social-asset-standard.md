# Never Coming Soon
## Social Asset Standard v1.1

## 1. Purpose

This standard governs the canonical social handoff produced after a Never Coming Soon article is final.

The asset system should not reinterpret the production from scratch.

Generation develops the production and prepares the campaign handoff. The image workflow executes that handoff.

Core doctrine:

**NCS owns the architecture. The production owns the art direction.**

## 2. Canonical handoff

Every successfully delivered production receives one `ig_packet_json` object.

The packet is built from:

- frozen final canon
- final public article
- final title, format, and genre
- final editorial understanding of the production

Do not build it from the original Ideation premise when Generation materially changed the production.

## 3. Canonical canvas

The default Instagram carousel canvas is portrait 4:5.

Delivery target:

- 1080 x 1350 pixels per slide
- all three slides use the same dimensions
- keep critical copy, logo, title, rating, and billing elements comfortably inside the crop-safe area
- leave approximately 7 percent of width and height as a default critical-content safety margin unless the composition clearly demands otherwise

The image-generation stage may work at the closest supported generation size, but deterministic finishing should deliver the canonical 4:5 crop.

Do not allow Slide 1 to use one aspect ratio and Slides 2 or 3 another merely because a generated background arrived differently.

## 4. Locked carousel spine

The canonical carousel always contains exactly three slides.

The content slots do not change.

Styling, palette, typography, imagery, composition, texture, and campaign concept may change radically by production.

### Slide 1: Cover / Poster

Required content:

- `Never Coming Soon presents`
- final title
- hero key art
- tagline
- rating
- billing block

Its job is to stop the scroll and create anticipation.

It should feel like real advertising art for an unreal production.

### Slide 2: Premise

Required content:

- final title
- premise header
- premise body copy
- one restrained visual motif

Its job is clarity.

A reader should understand who the production follows, the situation, the recurring or central dramatic engine, and why it is interesting in roughly 10 to 15 seconds.

### Slide 3: NCS Close

Required content:

- approved NCS logo asset
- short Hollywood-flavored personality line
- `THE FULL STORY IN NEVER COMING SOON`
- `LINK IN BIO`

Its job is brand recognition and continuation.

It should be the most minimal of the three slides.

## 5. Slide 1 poster standard

The cover needs a real campaign idea.

Avoid a generic literal scene when a more singular image, object, composition, graphic idea, or visual tension could carry the poster.

The poster should communicate genre and desire before the reader studies details.

Required text slots should remain stable, but their placement may respond to the composition.

Do not add format, genre, runtime, synopsis copy, CTA language, social labels, or NCS logo to Slide 1 unless future governance explicitly changes the spine.

`Never Coming Soon presents` appears as simple text, not as the master NCS logo.

The rating should feel plausible for the final production.

The billing block should look like poster furniture rather than a paragraph.

The poster title and tagline must remain readable at phone-feed scale.

The rating and billing block may be smaller, but should remain visibly intentional rather than disappearing into decorative noise.

## 6. Slide 1 visual hierarchy

Default hierarchy:

1. hero idea / key art
2. title
3. tagline
4. `Never Coming Soon presents`
5. rating and billing block

The hero image should remain intelligible when viewed quickly on a phone.

A poster is not successful merely because it accurately depicts the premise.

Ask:

**What is the one image from this campaign somebody might remember tomorrow?**

## 7. Slide 2 premise standard

Slide 2 is not a second poster.

It should normally be cleaner, quieter, and more legible than Slide 1.

The premise header should be an actual hook or opening line, not a generic label such as:

- The Premise
- About the Movie
- The Story
- What's It About?

The header should help the reader enter the production.

The body copy should be written specifically for social. Do not paste the raw Ideation premise or an arbitrary article paragraph.

Typical body target: roughly 65 to 110 words.

It may be shorter when the premise is exceptionally clean.

The copy should explain setup and engine before flavor details.

The text should usually occupy one obvious reading path. Avoid scattering premise copy into multiple competing blocks merely to make the layout look designed.

Do not turn Slide 2 into:

- a synopsis
- a cast list
- a character dossier
- a quote card
- an infographic
- a list of slogans
- a dense lore page

## 8. Slide 2 copy discipline

The premise copy is exact public copy. The image workflow should place it verbatim rather than rewriting it.

It must:

- match final canon
- use no em dash character
- avoid internal editorial terminology
- avoid spoilers beyond the public article's approved reveal policy
- avoid claims that real actors participated in the production
- read naturally on its own

If the exact copy does not fit the canonical canvas cleanly at an accessible size, the problem should be fixed in the asset packet copy before layout rather than by shrinking text into illegibility.

## 9. Slide 3 close standard

Slide 3 is the production campaign signed by Never Coming Soon.

It is not a generic corporate card pasted onto the end.

Use the exact approved NCS logo geometry or approved exported asset.

Do not regenerate or approximate the logo when deterministic compositing is available.

Logo color may adapt completely to the production campaign.

Do not default to NCS orange, navy, or cream when those colors weaken the campaign.

The fixed content slots are:

- approved NCS logo
- one Hollywood/personality line
- `THE FULL STORY IN NEVER COMING SOON`
- `LINK IN BIO`

Do not add a second poster, synopsis, slogan stack, or unrelated brand copy.

`LINK IN BIO` should be the dominant action phrase on the slide without overwhelming the logo or Hollywood line.

## 10. Hollywood line

The Hollywood line is an NCS personality beat, not a second production tagline.

It should lightly acknowledge the central joke that this desirable thing does not exist.

Good territory:

- Hollywood missed this one.
- Apparently Hollywood forgot this one.
- Your move, Hollywood.
- Someone tell Hollywood.
- Hollywood, feel free to steal this one.

The exact line should fit the production and should not become hostile, bitter, or repetitive.

Do not substitute a thematic movie line such as `Sometimes the hardest call benches your own kid.` That belongs to poster copy, not the NCS close.

## 11. Campaign coherence

All three slides should feel like one campaign.

Shared elements may include:

- palette
- title typography family
- one recurring motif
- framing device
- texture
- lighting logic
- graphic language

Do not repeat every motif on every slide.

The cover can be rich. The premise should breathe. The close should be elemental.

## 12. Logo treatment

The packet must contain a concise `logo_treatment` instruction.

It should specify how the approved logo should be recolored or placed for this production.

The instruction governs color and presentation, not logo geometry.

Never invent a new logo shape as part of production art direction.

## 13. Caption

Every `ig_packet_json` must include one exact `caption` field.

The caption is social copy, not image copy.

Default target:

- 1 to 3 short sentences
- roughly 15 to 45 words
- punchy enough to work without the article
- specific to the production
- different from Slide 2 body copy

The caption may use the title, premise hook, or one irresistible detail.

It should make somebody want to swipe or open the full story.

Do not default to generic engagement bait such as `Would you watch this?` or `Thoughts?`.

Do not include hashtags by default. A growth layer may add platform-specific hashtags separately when useful.

Do not include emojis by default.

Do not imply real cast members endorsed, joined, announced, or participated in the fictional production.

No backstage technology language.

No em dash character.

## 14. Image prompt standard

Each slide has an execution-ready `image_prompt`.

Prompts should specify the visual object, composition, mood, lighting, palette, negative-space needs, and important story-world details.

They should not ask the image system to decide the premise, title, campaign concept, or copy hierarchy from scratch.

Prompts should anticipate the 4:5 crop and reserve useful negative space for the required text where appropriate.

When text fidelity matters, prefer generating the visual background and compositing exact typography and the approved logo deterministically afterward.

## 15. Deterministic finishing

The final social asset should normally separate image creation from exact graphic assembly.

Preferred order:

1. generate or source the campaign background/key art
2. crop and position to the canonical 4:5 canvas
3. place exact title, tagline, premise copy, rating, billing block, Hollywood line, CTA, and approved NCS logo deterministically
4. verify visual hierarchy and safe margins
5. export all three slides at identical dimensions

Do not ask an image model to recreate the master logo or long exact premise copy when a deterministic compositor can place them accurately.

## 16. Real actors and key art

Casting may use real performers in the editorial article.

The social campaign must never imply that those performers actually signed onto, endorsed, announced, or participated in the fictional production.

Key art does not need recognizable actor likeness to succeed.

Prefer concept-led campaign imagery when actor likeness would make the asset feel like a false real-world announcement or create unnecessary visual-fidelity problems.

## 17. Packet quality test

Before the packet is persisted, ask:

- Does Slide 1 have one memorable campaign idea?
- Is the poster still legible as a poster rather than a premise card?
- Does Slide 1 work quickly at phone-feed scale?
- Does Slide 2 explain the actual production quickly?
- Is the Slide 2 header a hook rather than a label?
- Is Slide 2 copy final and placement-ready?
- Can Slide 2 fit legibly on a 4:5 canvas without shrinking into fine print?
- Is Slide 3 unmistakably the NCS close?
- Is the Hollywood line actually about the NCS/Hollywood conceit?
- Is the approved logo geometry preserved?
- Does logo color fit the production rather than defaulting to house colors?
- Do all three slides feel like one campaign?
- Is the caption short, specific, and non-redundant?
- Is all public copy free of em dashes and internal workflow language?

If not, the packet is not ready.
