# Designing pages that look high-end

Polish comes from restraint and rhythm, not decoration: one clear focal point per section, consistent spacing, real content and one accent colour. Use this with the page structure and copy rules (`writing.md` in frontlit-pages, or the checklist in this skill's `SKILL.md`).

## 1. Start from the product's own design system

Match the product's real tokens (colours, fonts, radius) in the site theme before touching any section (`theme-from-design-system.md`). Never invent ad-hoc colours per section; every colour on the page should trace back to a token, or to the single brand accent.

## 2. Hierarchy

- **One H1 (the hero title) and one focal point per section.** Each section answers one question; its title is the answer in under ~10 words.
- **Type ladder:** H1 ≫ H2 > card titles > body. Leave the theme's sizes alone unless the hero needs to be bigger. On mobile, keep H1 ≤ 40 px (2–3 lines at 390 px) and body ≥ 16 px.
- **Eyebrow + title + one-line lead** is the standard section header. Eyebrows are short, sentence case or small caps, in the accent or muted colour.
- **Highlight one phrase per title at most** (`*asterisks*`), and not on every section.
- **Measure:** prose and leads at 60–75 characters per line. Use a narrow `maxWidth` (`max-w-3xl`) for text-heavy sections, and the page width for grids and tables.

## 3. Rhythm and layout

- **A sequence that breathes:** hero (big, `py-24`+), proof strip (tight, `py-8`/`py-10`), 2–4 content sections (theme default), one contrasting band, FAQ (narrow), closing CTA band.
- **Alternate density:** a dense grid, then an airy content block, then a table. Don't stack three card grids in a row.
- **Zig-zag** consecutive `content` blocks (media left, then right).
- **Fill rows:** card counts that fill rows (3 or 6 in three columns; 2 or 4 in two). Keep card copy similar in length so cards line up.
- **Backgrounds:** one tinted or textured band in the middle and a contrasting closing band are enough. A different background on every section reads as noise.
- **Consistent corner radius, border weight and icon style** across blocks and embeds (use the theme radius in embeds too).

## 4. Hero legibility (non-negotiable)

- **Contrast:** text contrast ≥ 4.5:1 (body) and ≥ 3:1 (large headings) against the *actual* pixels behind them, in light and dark.
- **Texture behind text:** keep texture and glows behind text low-contrast. Push them to the edges with a mask (`radial-gradient(ellipse at center, transparent 30%, black 75%)`), or add an overlay (Background → Effects → Overlay, opacity 2–5).
- **No busy image behind a headline** without an overlay. Prefer the image beside the text (`normal` layout) or below it (`media-below`).
- **Check 390 px first.** Glows positioned for desktop often land behind the headline on mobile. The title shouldn't wrap one word per line, and buttons should be full-width or side by side, never overflowing.
- **Motion:** never animate the hero text itself.

## 5. Section backgrounds (Pattern recipes)

In the builder: select the block, then **Background → Pattern**. Pick a preset (Tiles or Glows), or paste your own into **Custom CSS**, which has separate **Light mode** and **Dark mode** values. Under **Advanced** you can set size, position, repeat and a mask.

In JSON: `"background": { "type": "gradient", "backgroundImage": "…", "backgroundImageDark": "…", "gradientBackgroundSize": "…", "gradientBackgroundRepeat": "…", "gradientMaskImage": "…" }`. Escape inner double quotes as `\"`.

**How layers work:**
- Layers are a comma-separated CSS `background-image` list; the first layer is on top.
- Size and position accept comma lists, one entry per layer. Repeat is a single value for all layers.
- The value can use page variables (`var(--background)`), because the layer sits inside the themed page.
- Pasting into the builder field works with quotes as-is.

Tested recipes (swap the violet `155,120,214` / `177,145,234` for your accent):

- **Film grain plus soft glows.** Repeat: `repeat`. Size: `160px 160px, 100% 100%, 100% 100%`.
  ```css
  url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='g'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.85' numOctaves='3' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23g)' opacity='.14'/%3E%3C/svg%3E"), radial-gradient(60% 50% at 20% 0%, rgba(155,120,214,.30), transparent 70%), radial-gradient(50% 45% at 90% 20%, rgba(214,170,120,.22), transparent 70%)
  ```
- **Mesh blobs.** Size `100% 100%`, no-repeat.
  ```css
  radial-gradient(40% 50% at 15% 20%, rgba(155,120,214,.28), transparent 70%), radial-gradient(35% 45% at 85% 15%, rgba(120,170,214,.22), transparent 70%), radial-gradient(45% 55% at 60% 100%, rgba(214,150,190,.20), transparent 70%)
  ```
- **Eclipse arc.** A glowing horizon that leaves through the sides; the inside matches the page, so the next section continues it.
  ```css
  radial-gradient(ellipse 127% 99% at 50% 152%, var(--background) 0 69%, rgba(177,145,234,.9) 70%, rgba(177,145,234,.25) 72%, transparent 78%)
  ```
  Put it in the layer list *above* any grain, so the inside is clean.
- **Light beam.** Mask: `linear-gradient(black 40%, transparent)`.
  ```css
  conic-gradient(from 160deg at 50% -15%, transparent 0deg, rgba(177,145,234,.22) 16deg, rgba(255,255,255,.28) 20deg, rgba(177,145,234,.22) 24deg, transparent 40deg)
  ```
- **Holographic band.** Mask: `linear-gradient(transparent, black 30%, black 70%, transparent)`.
  ```css
  linear-gradient(100deg, transparent 15%, rgba(255,120,220,.16) 32%, rgba(120,210,255,.18) 50%, rgba(255,230,140,.16) 68%, transparent 85%)
  ```
- **Slowly drifting glow (SVG + SMIL).** Size `100% 100%`.
  ```css
  url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100' preserveAspectRatio='none'%3E%3Cdefs%3E%3CradialGradient id='d' r='.5'%3E%3Cstop offset='0' stop-color='%239b78d6' stop-opacity='.5'/%3E%3Cstop offset='1' stop-color='%239b78d6' stop-opacity='0'/%3E%3C/radialGradient%3E%3C/defs%3E%3Ccircle cx='30' cy='40' r='45' fill='url(%23d)'%3E%3Canimate attributeName='cx' values='30;70;30' dur='40s' repeatCount='indefinite'/%3E%3C/circle%3E%3C/svg%3E")
  ```
- **Floating file-type tiles (SVG + SMIL).** Size `360px 240px`, repeat. Use `animateTransform` translate values like `0 0;0 -8;0 0` over 9–11 s per tile; see the drifting glow for the encoding.

**Encoding:** in a data URI, write `#` as `%23` and `%` as `%25`, and use single quotes inside the SVG. A background's SMIL animation ignores `prefers-reduced-motion`, so keep it slow (≥ 20 s), small in amplitude and low in contrast. For anything more, use an Embed with CSS animation and a reduced-motion fallback.

**Always set the Dark mode value too.** Typically you raise the glow alpha a little and lower the grain opacity.

## 6. Proof, diagrams and tables

- **Logo and tool strips:** the Marquee block shows text and Lucide icons only, with no images. For real logos, use an Embed: a row (or CSS-animated track) of inline SVG pills with a **fixed width** each (e.g. 160 px), so gaps are even. Colour them with `currentColor` from a page variable, and add `:host(.dark)` rules. Duplicate the track for a seamless loop, pause on hover, and stop under `prefers-reduced-motion`. Only use logos you're allowed to show.
- **Animated diagrams** (e.g. "file → API → CDN → app"): an Embed with inline SVG or HTML boxes, animated with CSS keyframes. Stack vertically below ~640 px container width. Make it clear without the animation (labels on every node).
- **Comparison tables:** an Embed with a real `<table>` (or tabs per competitor, see `comparison-pages.md`).
  - The first column is the feature; your product's column is lightly tinted with the accent.
  - Use ✓ / – / text, not colour alone. Let the table scroll inside its own box on mobile, or turn rows into cards there.
- **Code samples:** monospace from the theme, the same font size across samples, line counts measured not guessed. Show side by side on desktop, stacked on mobile.
- Follow `embeds.md` for every snippet, and run `scripts/test-embed.mjs` before pasting.

## 7. Motion

Purposeful motion is welcome: a diagram that shows the flow, a slowly drifting glow, a logo strip. Rules:
- Respect `prefers-reduced-motion`.
- Nothing essential should depend on motion.
- No motion on text people are reading.
- Usually no more than two moving things in view at once.

## 8. Final visual QA

Screenshot every page at 1280 and 390 px in light and dark. Check:
- Is the headline legible in every combination?
- Is anything clipped or overflowing horizontally?
- Is spacing between sections consistent?
- Does every colour come from the theme or the one accent (no stray gold or amber, for example, when the brand is violet)?
- Is there any demo copy or placeholder left?
- Do images have alt text?
- Do header and footer links work?
