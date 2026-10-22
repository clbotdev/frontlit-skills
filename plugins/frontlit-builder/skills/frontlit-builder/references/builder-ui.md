# The FrontLit builder: UI, quirks and automation

Labels below are FrontLit's at the time of writing. If a label differs, look for the nearest equivalent, and report the change (see "Reporting quirks").

## Before you start

- **Check the team.** The dashboard often opens on another team the user belongs to. A page URL under the wrong team shows "Page not found" instead of offering to switch (FrontLit #104). Switch to the site's team first (the team switcher in the dashboard), then open the site's pages.
- Note which pages already exist and what's published. Everything you change stays a **draft** until someone clicks **Publish**.

## Pages

- **New pages automatically get the site Header and Footer.** These are shared blocks: editing them changes every page. The Footer has **Link Columns**, **Copyright** and a **color-mode toggle**.
- **Changing a page's slug changes its public URL immediately,** even before you publish the content. Settle the slug before sharing links, then update header and footer links and any links on other pages.
- The **top bar** has:
  - **Preview device** (Mobile / Tablet / Desktop)
  - **Edit theme**
  - **Edit SEO settings**:
    - Search results: **Title**, **Description**, **Search engine visibility**
    - **Social card image**: 1200×630; choose one or paste a URL
  - **Add a new block to the page**
- **Blocks:** click a block's outer area to open its settings. Clicking inside rich text or an embed may not open them. The block toolbar has **Move up**, **Move down** and **Delete**. Delete asks for confirmation ("Delete <block>?") and removes the block from the draft only.
- **Section background:** block settings → **Background**: Color / Image / Pattern.
  - Pattern has **Presets** (Tiles, Glows), **Custom CSS** (Light mode / Dark mode) and **Advanced** (size, position, repeat, mask).
  - Every block also has padding, width and a CSS id (anchor).
- **Embed block:** **Type** is **URL** or **Script / iframe snippet**, the content goes in **Snippet**, and **Aspect ratio** should usually stay **Default (fit content)**.
  - In the editor a transparent overlay covers the embed, so its tabs and buttons can't be clicked there (#110). Test them locally; see `embeds.md`.

## Rich text and articles

- **Pasting raw Markdown produces a code block,** not formatted text. Convert Markdown to HTML first (any Markdown converter), then paste it as HTML. From a script, dispatch a `paste` `ClipboardEvent` whose `clipboardData` has `text/html`. By hand, copy the rendered HTML from a browser tab.
- **Links:** select the text and use the toolbar's **Insert link** button. **Ctrl/Cmd+K doesn't open the link dialog.**
- **Articles (blog posts) have Title and Excerpt only. There's no SEO field;** the excerpt is the search and social description, so write it like a meta description (120–160 characters). Pages have **Edit SEO settings** instead.
- **Legal pages (terms, privacy):** copy the product's real, existing terms and privacy text **verbatim** (from the current site, the repository or the user). Never write, summarise or "improve" legal text. If none exists, stop and ask. Publish terms and privacy together so they never contradict each other.

## Big snippets and copying

- **There's no character limit:** snippets of ~71k characters paste and save fine.
- Copy reliably from a **local helper page** with a Copy button, rather than from a terminal or an editor that might wrap or alter the text:

  ```html
  <!doctype html><meta charset="utf-8"><title>Copy snippet</title>
  <button id="c">Copy</button> <span id="s"></span>
  <textarea id="t" style="width:100%;height:80vh">PASTE OR LOAD SNIPPET HERE</textarea>
  <script>
  c.onclick = async () => { await navigator.clipboard.writeText(t.value); s.textContent = t.value.length + " characters copied"; };
  </script>
  ```

  Generate it with the snippet inlined (escape `</textarea>`). Open it via `file://` or `localhost`, click **Copy**, and paste into **Snippet**. Compare the character count with the source file.

## Driving the UI from automation (until there's an API)

Prefer the FrontLit MCP tools (`list_pages`, `get_page`, `update_page`…) or Git sync (frontlit-pages skill) when available. When you must drive the builder:

- **Inputs** are React-controlled. Set the value with the native setter, then fire events:

  ```js
  Object.getOwnPropertyDescriptor(HTMLTextAreaElement.prototype, "value").set.call(el, text);
  el.dispatchEvent(new Event("input", { bubbles: true }));
  el.dispatchEvent(new Event("change", { bubbles: true }));
  ```

  Use real clicks for Save, Done and Publish-adjacent buttons.
- **Rich text (TipTap):**
  - To replace content, select an explicit DOM range over the editor's content, then paste a `ClipboardEvent` with `text/html`. "Select all" may not select everything.
  - To change one phrase, select just that text node and use `insertText`, so the surrounding formatting survives.
  - Wait ~2 s after an edit before clicking **Done**, or the change can be lost.
- **Reloads drop helper functions you injected;** reinstall them. After any reload, check that each change persisted.
- **Never click the media library's "Remove".** It deletes the file permanently, even if a published page uses it.
- **Never click the header logo in the canvas.** It can navigate away from the editor.
- **Never click a theme's ✓ / "Use" unless asked.** It applies the theme to the whole site; see `theme-from-design-system.md`.

## Review without a draft preview link

There's no shareable draft preview URL yet (FrontLit #103), and the editor canvas isn't the live render: embeds are overlaid, and theme colours such as `--primary` can differ in the preview. So:

1. Test every embed locally with `scripts/test-embed.mjs` (1280 / 390, light / dark, shadow root).
2. Take screenshots of the page in the builder at desktop and mobile (Preview device), in light and dark where possible.
3. Send them to the user with the list of changes, and wait for approval.
4. **Publish only when told.** Then open the live URL at 1280 and 390 in both modes, click through tabs and links, and confirm.

## Reporting quirks

Keep a short log of anything surprising: wrong team, paste behaviour, an overlay, missing settings, labels that changed. Report it to whoever maintains FrontLit, with steps and screenshots.

Known so far:
- **#104:** a page URL under the wrong team shows "Page not found" instead of offering to switch.
- **#103:** there's no draft preview link.
- **#110:** embed snippets can't be interacted with in the editor.
- Ctrl+K doesn't open the link dialog.
- Pasted Markdown becomes a code block.
- The editor preview's `--primary` differs from the live theme.
