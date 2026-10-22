---
name: frontlit-builder
description: Build and edit FrontLit landing pages, articles and legal pages in FrontLit's visual page builder (the browser editor), including themes from a product's design system, high-end section design, custom Embed snippets (diagrams, comparison tables, tabs) and comparison pages. Use when the site's pages are edited in the builder rather than synced from Git, or when designing embeds and themes for either. For pages kept as JSON in a repository use frontlit-pages; for docs use frontlit-docs.
---

# FrontLit Builder

FrontLit's page builder edits a site's pages in the browser: blocks (hero, grid, pricing, FAQ, embed…), a site-wide theme, a shared header and footer, and articles. Your job: pages that look like the product's own design, read well on a phone, contain only true claims, and stay drafts until the user says publish.

Read these as needed:

- `references/builder-ui.md`: the editor itself, its quirks, rich text and articles, copying big snippets, automation tips, review flow. **Read before your first edit.**
- `references/theme-from-design-system.md`: turn the product's design tokens into the FrontLit theme (`scripts/oklch-to-hex.mjs`).
- `references/design.md`: hierarchy, rhythm, hero legibility, tested background recipes, logo strips, diagrams, tables, visual QA.
- `references/embeds.md`: **how Embed snippets really run** (inline, shadow root, scripts run twice, no `currentScript`), the rules, a template, and `scripts/test-embed.mjs`. **Read before writing any snippet.**
- `references/comparison-pages.md`: tabbed "X vs alternatives" pages with measured, verified rows.

## Ground rules

- **Everything stays a draft. Never publish, unpublish or delete a published page unless the user explicitly asks.** SEO settings and theme drafts also go live on Publish.
- **Never click a media "Remove" button** (it permanently deletes the file, even if a live page uses it). **Never apply a theme (✓ / "Use") unless asked** (it restyles the whole site).
- **Switch to the right team first** (`builder-ui.md`, FrontLit #104).
- **Only true content.** Don't invent customers, logos, numbers, quotes, prices or legal text. Unverifiable comparison facts are written "Unclear". Legal pages copy the product's real text verbatim.
- **Fonts and scripts:** no Google Fonts or other web-font links (they cost PageSpeed on every page); inherit the theme's fonts. No third-party scripts in embeds.
- **Review before publishing:** send desktop and mobile screenshots (light and dark) plus a change list, and wait. Report builder quirks to whoever maintains FrontLit.

## Workflow

1. **Orient.** Open the right team and site, and list the existing pages, header, footer and theme. Ask which page to work on, its slug, and the one action it should drive.
2. **Theme.** If the product has a design system and the user wants the site to match it, map its tokens (`theme-from-design-system.md`). Don't invent per-section colours.
3. **Plan the page.** Write down the visitor, the action, the top three objections, and a section list where each block has one job:
   - hero, then proof, then what it does, then how it works or depth, then pricing, FAQ and a closing CTA
   - use `design.md` for rhythm and backgrounds
4. **Build with blocks first.** Set every visible text: unset fields show FrontLit's demo copy. One H1 (the hero); every other title is H2. Give linked sections a CSS id.
   - **Pricing** has no monthly/yearly switch. Write the period in the price or the description ("$10/mo or $100/yr").
   - **Marquee** shows text and icons only. For logo images use an Embed (`design.md`).
5. **Embeds for what blocks can't do** (diagrams, tables, tabs, code side by side). Follow `embeds.md`, run `scripts/test-embed.mjs`, look at all screenshots, then paste.
6. **SEO.** Under **Edit SEO settings**:
   - **Title:** 50–60 characters, the promise first.
   - **Description:** 120–160 characters.
   - **Search engine visibility:** on, unless it's a private page.
   - **Social card:** 1200×630.

   For articles, the Excerpt is the description.
7. **Review.** Do the visual QA in `design.md`, then send screenshots (desktop and mobile, light and dark) and wait for approval.
8. **Publish only when told.** Then check the live page at 1280 and 390 in both modes, including interactive embeds.

## Copy checklist

- The hero sells an outcome and a concrete scene, not a list of integrations. Its sub-copy says how and for whom.
- Show proof (who uses it, real numbers) high on the page.
- Frame developer-only sections by situation ("Adding uploads to your app? Skip the plumbing"), after the section everyone understands.
- Explain product jargon the first time it appears.
- The FAQ answers a newcomer's real objections: limits, privacy, lock-in, "is this just a wrapper?".
- Verify claims against the product's code or docs. Hosted and self-hosted capabilities often differ.
- Buttons are verb + object ("Start free"). Cut adjectives any competitor could use.
