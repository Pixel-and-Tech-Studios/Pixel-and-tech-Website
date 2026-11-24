# details.md — project notes for Pixel and Tech Studio website

> Working notes kept on the `my-changes` branch only. Never merge this file into `main`.
> Read this first at the start of every session, and update it whenever a change makes it out of date.
> Last updated: 24 November 2026 (icon theme toggle with saved preference, Discord icons with "coming soon" tooltip).

---

## 0. Working rules (agreed with the owner)

- Work only on branch **`my-changes`** (created from `main` at commit `341d6bb`).
- Never commit to, push to or merge into `main`. No pull requests or merges unless explicitly asked.
- Check `git branch --show-current` says `my-changes` before any change.
- Push with `git push -u origin my-changes` (the branch must track `origin/my-changes`, never `origin/main`).
- No `CLAUDE.md` file.
- After each pushed change, give the owner the **branch preview link** (not GitHub, not the main site).
  Preview link: https://pixel-and-tech-preview.netlify.app (see section 9). Check it shows the new commit before sharing.

---

## 1. Purpose

The public marketing website for **Pixel and Tech Studio**, a small startup game studio and software company in
Newcastle upon Tyne, UK. It is aimed at:

- **Players**: play a mini game (Pixel Catch), browse and download the studio's games, join Discord, sign up to news.
- **Businesses**: learn about custom software, web design and an AI call-handling ("AI receptionist") service, and send an enquiry.
- **Job seekers**: leave details even though the studio isn't hiring.
- **Press**: press kit page with boilerplate text, logo, colours and fonts.

Contact email used everywhere: `pixelandtech.uk@gmail.com`.

## 2. Tech stack

| Part | What is used |
|---|---|
| Language | Plain HTML5, CSS and vanilla JavaScript (ES5 style: `var`, `function`, no modules) |
| Frameworks | **None**. No React, no npm, no `package.json`, no build step |
| Fonts | Google Fonts: Bricolage Grotesque (headings) and Figtree (body), loaded by `<link>` |
| Graphics | `<canvas>` (drawing pad, mini game) and inline SVG generated in JS (icons, placeholder screenshots) |
| Storage | Browser `localStorage` only (game scores, player name, light/dark choice) |
| Hosting | GitHub Pages, deployed from `main` |
| Special | Optional `window.claude.use('downloads')` hook: only works when the page runs as a Claude artifact; on GitHub Pages those press-kit download buttons stay hidden |

There are no third-party libraries or versions to track.

## 3. Structure

```
README.md    2 lines, title and one-line description
TODO.md      Launch to-do list written for the owner (content, legal, hosting, nice-to-haves)
index.html   THE ENTIRE WEBSITE (~1,300 lines): CSS in <style>, HTML, JS in one <script>
details.md   This file (my-changes branch only)
```

Inside `index.html`, in order:

| Lines (approx.) | Contents |
|---|---|
| 1–15 | `<head>`: meta, title, description, Google Fonts, tiny inline script that applies the saved theme before paint |
| 11–470 | `<style>`: design tokens on `:root`, dark mode, then CSS per section (header, hero, ticker, games, cards, services, AI demo, library, process, about, wipe transition, product pages, carousel, loader, tiles, newsletter, leaderboard, policy/press pages, hiring, FAQ, contact, footer, mobile `@media (max-width:900px)`, reduced motion) |
| 473–505 | Loader, page-wipe overlay, sticky header with nav, Discord, theme and Menu buttons |
| 507–689 | `<main id="top">` with all home sections: hero + pixel pad, ticker, tiles, `#games`, `#community` (Discord), `#services`, `#ai`, `#software`, `#process`, `#about`, `#hiring`, `#faq`, `#news`, `#contact` |
| 691–693 | `#viewApp` — empty container filled by JS for each product page |
| 695–722 | `#viewPrivacy` — privacy policy page |
| 724–774 | `#viewPress` — press kit page |
| 776–791 | Footer |
| 793–1295 | `<script>` (see section 5) |

## 4. How it runs

- **Install**: nothing to install.
- **Run locally**: open `index.html` in a browser, or serve the folder, e.g. `python3 -m http.server 8000` and visit `http://localhost:8000`.
- **Build**: none.
- **Test**: no automated tests, no linter, no CI except GitHub's automatic Pages workflow.
- **Deploy**: anything merged/pushed to `main` is published by GitHub's "pages build and deployment" workflow
  (run #1 failed, run #2 succeeded on 20 Nov 2026 for commit `341d6bb`).
- **Environment variables / config**: none. The only "config" is in the script:
  - `APPS` list (top of the script) — every game/app/tool.
  - `DISCORD_URL` — Discord invite link (currently empty, so buttons say "coming soon").
  - The email address `pixelandtech.uk@gmail.com` is hard-coded in ~8 places (HTML links, forms JS, fact sheet).

## 5. Architecture and data flow

It is a **single-page app with hash routing**, no backend.

- **Routing** (`route()` / `show()` at the bottom of the script): listens to `hashchange`.
  - `PAGES` maps a page key to the sections shown, e.g. `services: ['#services','#ai','#process']`. Every section listed in any page is hidden unless it belongs to the current page.
  - `ALIAS` maps hashes to page keys (`#careers` → `hiring`, `#faq` → `about`, …).
  - `#privacy` and `#press` show the separate page divs.
  - `#app-<slug>` (e.g. `#app-pixel-drift`) renders a product page from the `APPS` entry via `renderApp()`.
  - `#ai`, `#process`, `#faq`, `#news` scroll to that section inside its page.
  - Page changes play the coloured "wipe" animation (skipped with reduced motion).
  - `#news` (newsletter) and the footer are not in `PAGES`, so they show on every main page.
- **Software catalogue**: `APPS` array → `card()` builds cards for the Games list (type `Game`) and the filterable/searchable Software library (`renderLib()`), and `renderApp()` builds the product page (carousel, price, Buy / Download / Wishlist buttons, "More from us").
  Icons and placeholder screenshots are generated from a hash of the app name (`iconSVG`, `sceneSVG`), so they're stable but random-looking.
- **Forms** (contact, careers, newsletter): no server. On submit they build a `mailto:` link and open the visitor's email app. Nothing is sent unless the visitor presses send.
- **Pixel Catch game**: canvas game loop with `requestAnimationFrame`; scores saved to `localStorage`.
- **AI call demo**: plays a scripted fake conversation (`SCRIPT`) message by message. No real AI or phone service.
- **Theme**: light/dark tokens in CSS. With no saved choice, the CSS `prefers-color-scheme` media query follows the device.
  The sun/moon icon button sets `data-theme` on `<html>` and saves it to `localStorage["pt-theme"]`; an inline script in
  `<head>` re-applies it before first paint (no flash). The icon shows the mode you'll switch *to* (moon in light, sun in dark).
  Changes in another tab are picked up via the `storage` event.
- **Discord**: every Discord control is `<button data-discord>` filled by `wireDiscord()`: a pixel gamepad icon plus a
  "Discord: coming soon" tooltip (hover, keyboard focus or click/tap; Esc hides it; clicks announce via the `#dcLive`
  screen-reader region). Places: header (hidden under 360px wide), "Join our Discord" box, footer, product-page side box.
  When `DISCORD_URL` is set, each button is swapped for a real link that opens in a new tab.
- **Third parties**: Google Fonts, Gmail (via mailto), Discord (link only), GitHub Pages.
- **Auth / database / APIs**: none.

## 6. Data

| Data | Where | Shape |
|---|---|---|
| Products | `APPS` array in `index.html` | `{name, type:"Game"\|"App"\|"Tool", kind:"download"\|"web", platform, version, size, price, desc, long, features[], url, buyUrl, wishlistUrl, media[], buyLabel?, wishlistLabel?}`; `media` items `{type:"image",src,alt}` or `{type:"video",src,poster}` |
| AI demo script | `SCRIPT` array | `[speaker, text]` pairs, speaker `ai` / `caller` / `sum` |
| Best score | `localStorage["pt-best"]` | number as string |
| Top 5 scores | `localStorage["pt-scores"]` | JSON `[{n:name, s:score}]` |
| Last player name | `localStorage["pt-name"]` | string, max 12 chars |
| Theme choice | `localStorage["pt-theme"]` | `"light"` or `"dark"`; absent = follow device |
| Press fact sheet | `FACT_SHEET` string in JS | plain text |

No database. Nothing is stored on a server.

## 7. Conventions to follow

- **Keep it one file** unless the owner agrees otherwise (TODO.md says "no build step").
- **Colours**: always use the CSS variables (`--bg`, `--surface`, `--ink`, `--muted`, `--line`, `--magenta`, `--cyan`, `--sun`); dark values are defined twice (media query + `[data-theme="dark"]`) and both must be updated together. Brand hex values: magenta `#E4007C`, cyan `#00A9CE`, yellow `#FFC72C`, navy `#1B1A4B`, paper `#F6F7FC`.
- **Visual style**: "pixel/brutalist" — 2px solid ink borders, square corners (`border-radius:0`), hard offset shadows (`box-shadow:6px 6px 0 var(--c)`), per-item accent via `--c` and `:nth-child`.
- **CSS**: compact one-line rules, short class names (`.btn`, `.btn.ghost`, `.chip`, `.svc`, `.app`, `.tile`), grouped under `/* section */` comments. Mobile breakpoint 900px, small 560px. Respect `prefers-reduced-motion`.
- **JS**: ES5 (`var`, `function`, `forEach`), short names, IIFEs for one-off setup, `/* ---- section ---- */` comments, HTML built as strings — always pass user/content text through `esc()`.
- **New top-level page section**: add a `<section id="...">` inside `<main>`, then add its selector to the right `PAGES` entry and its hash to `ALIAS`, and a nav/footer link if needed.
- **Copy/wording**: British English, plain friendly language, short sentences, curly apostrophes (’).
- **Small icons**: draw them as pixel grids in `GLYPHS` and render with `glyph(name)` (SVG in `currentColor`); put them in `.icon-btn.ico` (40×40) buttons with an `aria-label`. Larger decorative icons use `ICONS` / `data-icon`.
- **Accessibility**: `aria-label`, `aria-pressed`, `aria-current`, `.sr` for screen-reader-only text, `:focus-visible` outlines.

## 8. Current state, issues and TODOs

**Recent commits** (only two, both 20 Nov 2026 by Professorxrx): "Initial commit" (README) and "Add files via upload" (index.html + TODO.md). The site was generated with Claude and uploaded through the GitHub web UI. No PRs, no other active work. A leftover branch `claude/great-wozniak-c1tv8d` (identical to main) exists on GitHub from session setup.

**Unfinished (from TODO.md and code):**
- All six `APPS` entries are **sample/placeholder** products (Pixel Drift, Block Keeper, Tidy Tasks, CallPilot Demo, Sprite Sizer, Invoice Snap); every `url`, `buyUrl`, `wishlistUrl` is empty, so every button says "Coming soon".
- `DISCORD_URL` empty (Discord icons show "coming soon").
- Forms only open the visitor's email app (no form service). Newsletter is not a real mailing list.
- AI demo uses a made-up plumber ("Hartley Plumbing", "Sam Patel, Exeter") — odd for a Newcastle studio.
- No favicon, no social preview (Open Graph) image, one title/description for all "pages".
- Privacy policy is a first draft and not legally reviewed. TODO.md still lists "create GitHub repo / turn on Pages" as not done, but both are now done.
- No custom domain yet.

**Inconsistencies / possible bugs:**
- Software library says "Everything we’ve released, free to download", and the press kit says "free to download", but Pixel Drift is priced £4.99.
- FAQ says each card shows "how big the file is"; web apps show "Runs in your browser" instead (fine, but wording slightly off).
- Hash routing means search engines see one page; product pages have no own URL for sharing previews.
- Press-kit download buttons never appear on GitHub Pages (by design).
- Loader is hidden twice (CSS at ~1.3s and JS at 1.4s) — harmless duplication.
- Hiring section reuses `.contact-grid` class; some CSS duplicated between `.app-foot .btn` and `.btn.sm`.
- `curPage` variable in the router is set but never read (dead code).

## 9. Deployment and previews

- **Host**: **GitHub Pages**, from the `main` branch (confirmed via the repo's "pages build and deployment" Actions runs).
- **Expected live URL**: `https://pixel-and-tech-studios.github.io/Pixel-and-tech-Website/` — *not yet confirmed from this environment (outbound network to that host is blocked here); the owner should check under Settings → Pages.*
- **Branch preview: set up on Netlify (24 Nov 2026).** Netlify project `pixel-and-tech-preview`, branch to deploy = `my-changes`,
  no build command, publish directory = repo root. Every push to `my-changes` redeploys
  **https://pixel-and-tech-preview.netlify.app** (confirmed serving `my-changes`: `details.md` loads there and is not on `main`).
  Netlify adds a small HTML comment and a script tag to the served page; the repo file is unchanged.
  Note: files in the repo, including this `details.md`, are publicly reachable at that URL.
- This session's cloud environment "Pixel-and-tech-Website" allows `pixel-and-tech-preview.netlify.app` so previews can be checked before sharing.
