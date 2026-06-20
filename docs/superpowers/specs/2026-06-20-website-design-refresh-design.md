# Website Design Refresh — Warm-White + Ink

**Date:** 2026-06-20
**Branch:** `redesign/warm-white-ink` (in `corgssant/roya-site`)
**Status:** Approved design, pre-implementation

## Goal

Bring the marketing site (`royaengine.com`) in line with the app's current
design system. The site is still on the old cool/purple brand; the app shipped
a warm-white + ink-neutral system (the Lovable refresh). Source of truth:
`~/Roya/app/app/src/index.css`. New ink/paper logo marks are supplied by Vlad.

Non-goal: changing copy, page structure, animations, or section order. This is a
palette + type + logo + asset pass, not a rebuild.

## Source-of-truth palette (from `app/app/src/index.css`)

| Token | Value | Role |
|---|---|---|
| `--background` | `#fcfaf6` | page / nav / cards (warm white) |
| `--secondary` / `--muted` / `--accent` | `#f0ece5` | tint surfaces, section banding |
| `--foreground-strong` | `#1a1a1a` | titles, ink |
| `--foreground` | `#3a3a3a` | body text |
| `--muted-foreground` | `#807a6e` | tertiary / helper (warm gray) |
| `--primary` / `--ring` | `#1a1a1a` | CTAs, links, focus (ink) |
| `--primary-foreground` | `#ffffff` | text on ink |
| `--border` / `--border-soft` | `#e2ddd4` / `#eee9e1` | dividers, card edges |
| `--destructive` | `#993333` | errors |
| `--chart-1` | `#1b3ede` | **data-viz blue only** (live bars) |
| status-success | bg `#ecf5ee` / text `#345e3d` / border `#cce6d5` | "met" / done |
| status-warning | bg `#fcf3df` / text `#7a4f10` / border `#ead7a3` | caution |
| status-danger | bg `#fae9e9` / text `#9f3232` / border `#eecfcf` | failure |
| status-info | bg `#eef3fb` / text `#2c4a73` / border `#cedaf0` | neutral info |
| Font | **Geist** (sans) + **Geist Mono** (raw codes only) | replaces Inter + JetBrains Mono |

**Governing rule (from app):** color carries only *valence* (good/bad/attention)
or *data-viz*. Everything decorative becomes ink or warm-gray. Green = met/done,
red = error, blue = live progress/chart bars. No purple anywhere.

## Approach

Add a `:root` design-token block (mirroring `index.css` variable names) to the
top of each HTML file's inline CSS, then replace every hardcoded color and font
with a `var(--token)` reference. This makes the site a faithful, re-syncable
mirror of the app system rather than a one-off find/replace. One file at a time;
animations and DOM untouched.

## Section treatments (home — `index.html`)

- **Hero** (`#1a0f2e` dark-purple → `--background` warm-white): light/airy.
  Ink headline (`--foreground-strong`), warm-gray subhead, ink CTA. Hero mosaic
  cards become bordered warm-white cards (`--border` + soft shadow), not floating
  on a dark field.
- **Section rhythm:** alternate `--background` ↔ `--secondary` tint bands
  (replaces white / `#fafafa`).
- **Closing CTA section** (`#cta`): the single dark dramatic moment — ink
  `#1a1a1a` background, paper text, paper-on-ink button. (Approved.)
- **Integrations / logo bar** (`#1a0f2e` → `--secondary` tint): swap ERP logos
  from `-white` variants to the dark/color/bw variants already in `erp logos/`
  (e.g. `SAP-Logo-bw.png`, `sap_logo_rgb_onwhite…`, `Shopify Logo.png`,
  `Intuit_QuickBooks_logo.svg.png`, `NetSuite-Logo.png`, `Oracle_logo.svg.png`,
  `Square,_Inc._logo.svg.png`, `dual-entry-logo.png`, `amazon-logo-transparent.png`,
  rillet svgs). Render muted/grayscale for a calm strip.
- **Live monitor / calculator demos:** progress bar = `--chart-1` blue while
  filling → status-success green when "met"; pills → `bg-status-*`; source-data
  chips keep `Geist Mono` (semantic, mirrors app `font-mono` for raw codes).
- **Nav:** now on warm-white, so the nav logo switches from `logo-white.png` to
  the new ink **"Roya" wordmark**. Mobile menu bg `#1a0f2e` → warm-white + border.
- **Focus ring:** `#7c3aed` → `--ring` ink.

## Type

- Load **Geist** (Google Fonts), drop Inter + JetBrains Mono.
- Money / figures: `font-variant-numeric: tabular-nums`.
- **Geist Mono** retained only for raw field-code chips.
- Marketing display sizes stay large (hero needs scale). We match the app's
  *palette and font*, not its dense 12/14px chrome rule (that's for data tables).

## Assets (full scope)

| Asset | Action |
|---|---|
| `favicon.svg` | Rebuild from new ink **R emblem** (light default; ink variant for dark/pinned). I can author the SVG directly. |
| `favicon-16/32.png`, `apple-touch-icon.png`, `icon-192/512.png` | Regenerate from new emblem. |
| `logo.png`, `logo-white.png` | New ink **"Roya" wordmark** (Vlad drops finals; HTML wired to filenames). |
| `emblem.png` | New R mark (Vlad drops final). |
| `og-image.png` | Regenerate as warm-white + ink (mirrors new hero). I can author. |
| `theme-color` meta (home + blog) | `#7c3aed` → `#fcfaf6` |
| `site.webmanifest` | `theme_color` → `#fcfaf6`, `background_color` `#ffffff` → `#fcfaf6` |

## Files in scope

1. `index.html` — home (largest; tokens + all sections + demos).
2. `blog/what-i-learned-running-licensing-at-dolby.html` — blog post (same token block + recolor).
3. `site.webmanifest`, `favicon.svg`, `og-image.png` — assets/meta above.
4. PNG logo/emblem/icon set — regenerate or wait on Vlad's finals.

## Preview & deploy safety

- All work on branch `redesign/warm-white-ink`.
- Preview locally via a static HTTP server (`localhost`) — Vlad reviews in browser.
- **Do NOT push to `corgssant/roya-site` or deploy to `royaengine.com`** until
  Vlad explicitly approves. Preview ≠ publish.

## Verification

- Visual: side-by-side against the app for palette/type fidelity; check hero,
  banding, ink CTA band, integrations bar, live demos, blog post, favicon, OG.
- Grep: no purple/cool-gray hexes remain except inside `var()` fallbacks; no
  Inter/JetBrains references.
- Responsive: mobile nav + hero on narrow viewport.
