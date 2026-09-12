# Never Coming Soon
## Internal Dashboard Visual Standard v1.0

## 1. Purpose

The Never Coming Soon dashboard is an internal creative control room, not a public marketing site and not a generic SaaS admin panel.

It should feel calm, editorial, cinematic, and fast to operate. The interface exists to help one editor steer Ideation, inspect the queue, launch Generation, add notes, and open finished drafts.

Function comes before decoration.

## 2. Visual principle

**Editorial studio console.**

The dashboard should feel like the operational side of the same brand that publishes Never Coming Soon, without pretending to be a movie poster.

Use the brand palette and typographic contrast with restraint. Avoid cinema clichés, fake film grain, sprockets, clapperboards, projectors, ticket motifs, velvet, stars, or excessive theatrical styling.

## 3. Palette

Canonical dashboard colors:

- Midnight Navy: `#08192F`
- Warm Ivory: `#F5EEDF`
- Reel Orange: `#F24B2C`
- Electric Cobalt: `#315CFF`
- Slate: `#202632`

Recommended usage:

- Midnight Navy: primary page background and strongest surfaces
- Warm Ivory: primary text and occasional light editorial surfaces
- Slate: cards, borders, secondary surfaces, and muted controls
- Reel Orange: primary action, active states, small emphasis, and status accents
- Electric Cobalt: secondary interactive accent used sparingly

Do not turn the palette into a rainbow of equal accents. Orange should feel intentional, not omnipresent.

## 4. Typography

Dashboard-specific typography may be more functional than public NCS creative assets.

Use:

- `Inter` for interface text, controls, tables, buttons, forms, and body copy
- `Instrument Serif` for the NCS wordmark treatment, major page headings, and occasional editorial emphasis
- system monospace for IDs only when useful

Use `next/font/google` or equivalent normal web loading. Do not bundle or expose font files manually.

The dashboard typography standard does not lock public Never Coming Soon poster typography.

## 5. Hierarchy

Prefer large clear headings, restrained labels, strong spacing, and obvious primary actions.

The eye should always know:

1. what can be entered
2. what can be run
3. what is currently happening
4. what is waiting in the queue
5. what is ready to read

Do not use dense data-dashboard composition when a clean editorial list works better.

## 6. Surfaces

Use subtle borders and spacing before heavy shadows.

Recommended:

- page background: Midnight Navy
- cards: slightly lighter navy/slate surfaces
- borders: low-contrast ivory or slate with transparency
- selected / active: modest Orange or Cobalt accent
- large input surfaces may use Warm Ivory with dark text when that improves focus

Avoid glassmorphism, exaggerated blur, gradients by default, neon glows, and dashboard chrome that competes with the content.

## 7. Components

Buttons should be compact and confident.

Primary action: Reel Orange background, readable high-contrast text.

Secondary action: transparent or Slate surface with restrained border.

Destructive or irreversible actions should require explicit confirmation and should never resemble the primary creative action.

Status pills should be legible but quiet. The status text matters more than the color.

## 8. Motion

Use minimal motion for loading, expansion, and state changes.

No decorative animation loops.

Creative work already carries enough personality. The dashboard should feel stable.

## 9. Responsive behavior

Desktop is primary. The layout should remain usable on tablet and mobile, but the design should optimize for a desktop browser where the editor can compare ideas and operate workflows quickly.

On narrow screens, stack major sections and preserve action visibility rather than shrinking everything into dense columns.

## 10. Brand posture

The dashboard should feel premium and specific without looking precious.

The public brand emotion is anticipation. The internal dashboard translation is controlled possibility: there should always be another idea to explore, another production to make, and a clear next action.