# Comparison pages ("X vs Y", "X vs the alternatives")

Visitors who land on a comparison page are close to a decision and suspicious of marketing. The page wins by being the most useful, honest comparison they can find.

## Structure

1. **Hero** (or a section header inside the embed): what's compared and the promise ("We put six upload services side by side: price, limits, code"). One CTA.
2. **Tabs, one per competitor** (an Embed; blocks can't do tabs). Each tab contains:
   - a short, fair intro: what the competitor is good at, and what it leaves to you
   - a **comparison table** (rows below), your product's column lightly tinted
   - **side-by-side code samples** doing the same task (e.g. "upload a file, get a URL") in each product
   - a one-line "choose them if… / choose us if…"
3. A **methodology note** under the tabs: when it was checked (date), currency, how setup time and lines of code were measured, and where to report a mistake.
4. **FAQ** for migration and lock-in questions, then a **closing CTA**.

Give each tab a key and support deep links (`/compare#s3` opens the S3 tab), so ads, docs and posts can link to one competitor. Keep the tab list wrapping (not scrolling) on mobile.

The single-competitor variant (`vs-x`) uses the same table and code samples without tabs.

## Rows that matter

Pick the rows a buyer would check, in this order. Drop a row you can't fill truthfully.

| Row | How to fill it |
| --- | --- |
| Signup required / free plan | From the pricing page, with limits ("1 GB, 50 MB max file"). |
| Paid plan price | The entry paid plan, with its unit ("$10/mo or $100/yr, 100 GB", "$0.023/GB-month plus requests"). |
| Max file size | From the docs, per plan. |
| **Setup time** | A conservative estimate for a developer new to the service, labelled "(est.)". |
| **Lines of code** | Count non-blank, non-comment lines of the real code samples on the page; same task, same language. |
| SDK needed | Whether a plain HTTP call works or an SDK and signing are required. |
| Browser/direct uploads, signed private URLs | Yes, no, or how ("presigned URLs you generate, plus CORS"). |
| **CDN included** | Whether delivery is through a CDN by default, or extra setup or cost. |
| **Egress / bandwidth** | What delivery costs (included, per GB, fair-use). |
| Image/video processing | Built in, add-on, or none. |
| **AI-agent access** | MCP server, CLI, or API keys an agent can use, as documented. |
| Self-hosting / open source | License and what self-hosting includes. |

## Claims policy

- **Only verified claims.** Check each cell against the competitor's current pricing page and docs, and keep the source URL and date in a notes file next to the snippet.
- When you can't confirm a fact, write **"Unclear"** (and optionally "not documented"), never a guess.
- Be fair. Say what the competitor does well. Never say "No" when the truth is "Yes, with setup"; write what the setup is.
- No competitor logos without permission; names in text are fine.
- Re-check prices and limits before every publish; they change often.
- Hosted and self-hosted capabilities often differ; label which one a cell describes.

## Build notes

- Build the whole comparison as **one Embed snippet** following `embeds.md`.
  - Prefix everything (`acme-`), and use page variables with fallbacks plus a hardcoded accent.
  - Use ARIA tabs (`role="tablist"`, `role="tab"`, `aria-selected`, `aria-controls`, arrow keys).
  - Use the find-your-root plus run-once script pattern.
  - Without JavaScript, show all panels; hide inactive ones only after the script runs (add a `acme-js` class).
- A 70k-character snippet is fine. Keep the source and code samples in files, generate the snippet from them, and re-run `scripts/test-embed.mjs` after every edit.
- **SEO:** title like "Acme vs S3, UploadThing and Cloudinary: price, limits, code"; the description names the competitors; the snippet's text is server-rendered and indexable.

## Worked example (MediaLit)

MediaLit's `/compare` page compares MediaLit with Amazon S3, UploadThing, Cloudinary, Filestack, Vercel Blob and Supabase Storage:
- tabs with deep links
- a table with signup, free plan, price, max file size, setup time (est.), lines of code (measured from the samples on the page), SDK needed, browser uploads, signed URLs, CDN, egress and AI-agent access
- side-by-side code
- a dated methodology note

All classes are prefixed `mlc-`, and the accent is a hardcoded violet.
