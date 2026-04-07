# Claude Code Brief: Add Teletext Studio to onurhan.github.io

## What this is

A standalone teletext art studio built in React. It lets visitors draw, generate AI-powered teletext art, animate it, and export PNG/video. It lives at a single URL: `onurhan.github.io/teletext`.

The studio is fully contained in one file: `teletext.html`. It uses React and Babel loaded from CDN — no build step, no npm, no dependencies to install.

---

## The file to add

`teletext.html` is attached alongside this brief. Add it to the root of the `onurhan/onurhan.github.io` repository.

**Target URL once deployed:** `https://onurhan.github.io/teletext.html`

---

## What to do

### Step 1 — Add the file

Copy `teletext.html` into the root of the repository (same level as `index.html`).

### Step 2 — Link to it from the existing site

There is currently a placeholder section for "Teletext Art" somewhere in the site. Find that section and replace the placeholder content (or link) with a proper entry point.

The link should go to `/teletext.html`. It can open in the same tab or a new tab — use `target="_blank"` if the site's existing links to external tools open in new tabs, otherwise keep it in the same tab.

Example minimal link:
```html
<a href="/teletext.html">Open Teletext Studio →</a>
```

Match the style of whatever existing navigation or card/link elements are already on the site. Do not add new CSS frameworks or change the site's existing design system.

### Step 3 — Verify

After pushing, confirm:
- `https://onurhan.github.io/teletext.html` loads and shows the studio
- The link from the main site navigates there correctly
- The page title reads "Teletext Studio — Sublatent"

---

## Important notes

**The AI Generate feature requires an Anthropic API key entered by the visitor.** The studio calls `https://api.anthropic.com/v1/messages` directly from the browser. This works fine for personal use — the visitor (Onur) enters his own API key when using it. There is currently no API key input UI in the studio; it uses the key from the browser environment. This is fine as-is.

**No Jekyll/Hugo config changes needed.** GitHub Pages will serve any `.html` file at root level directly. No frontmatter, no `_config.yml` changes required.

**No CSP issues expected.** The site is static HTML. If there happens to be a `Content-Security-Policy` header or meta tag blocking CDN scripts, it will need to allowlist `cdnjs.cloudflare.com` and `api.anthropic.com`.

**File size:** ~42KB. Fine for GitHub Pages.

---

## Summary of changes

| Action | File | Details |
|--------|------|---------|
| Add | `teletext.html` | The complete studio, drop in at repo root |
| Edit | existing page with teletext placeholder | Replace placeholder with link to `/teletext.html` |

That's it. Two changes total.
