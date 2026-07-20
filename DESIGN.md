# DESIGN SYSTEM: AaditPani-RVU Profile

A design system for a GitHub profile README, built around one idea: **an engineer's field notebook, not a hacker-movie dashboard.** Everything below exists to keep the page reading as one instrument instead of a stack of unrelated widgets.

---

## WHY THIS VERSION EXISTS

The previous pass (`v2025.1`) used a near-black background with cyan + amber + green + red all competing for attention, plus a badge wall where every technology badge carried its own brand color (PyTorch orange, TensorFlow orange, CUDA green, Docker blue...). Individually reasonable, together it reads as a pile of differently-colored rectangles with no shared voice — "pieces of paper stuck on a grey background."

`v2026.1` fixes this with **one accent color used everywhere** (not four), a **signature motif** (a thin contour/trace line, standing in for both topographic contour maps and an oscilloscope trace — ties to edge-hardware work and to the mountain photo in the profile) that recurs as the divider and background texture instead of a generic dot-grid, and far fewer bordered cards. Sections that didn't earn their place (fabricated geo/uptime stats, a mocked `curl` output, a WakaTime panel with no workflow behind it) were cut rather than restyled.

---

## COLOR PALETTE (Strict Reference)

| Role | Value | Usage |
|---|---|---|
| Background | `#0b0f14` | Primary page background — ink, not grey-black |
| Surface | `#121821` / `#0d1117` | Card/panel background gradient |
| Border | `#202b36` | Dividers, card outlines |
| **Accent (the only one)** | `#e0a458` | Links, highlights, active states, section accent bars — used consistently everywhere instead of a multi-color status system |
| Accent Soft | `rgba(224,164,88,0.14)` | Subtle fills, hover-equivalent backgrounds |
| Secondary Tint | `#3f5568` | Depth only — dividers, inactive/queued markers. Never used for "status" |
| Text Primary | `#e8ecf1` | Body text, content |
| Text Muted | `#7c8b9b` | Secondary text, metadata |
| Text Dim | `#48566a` | Labels, timestamps, least emphasis |

**Rules:**
- One accent color, full stop. Status is binary (accent = active/true, muted outline = inactive/planned) — never a 4-color traffic light (green/amber/graphite/cyan) again.
- Third-party badges (shields.io, github-readme-stats) must have their color params overridden to this palette. Never let a badge's brand color stand — that's what caused the "confetti" look.
- Never use pure white (`#ffffff`) for text — use `#e8ecf1`.
- Never use pure black (`#000000`) for background — use `#0b0f14`.

---

## SIGNATURE MOTIF: THE CONTOUR LINE

A thin, irregular horizontal line (2–3 stacked passes, varying amplitude) stands in for both a topographic contour (mountains, the profile photo's setting) and an oscilloscope/seismograph trace (systems, signal, edge hardware). It is the one recurring visual element that ties every section together.

- Used as the **section divider** in place of plain `---` rules and in place of the old dot-grid pattern.
- Rendered as a static `<path>` (no animation needed) in `Accent` at low opacity (0.5) plus `Secondary Tint` at lower opacity (0.25) offset slightly, to suggest layered contour bands.
- Appears at most once between major "acts" of the page (identity → proof of work → capability → contact), not between every subsection. Fewer dividers, not more — the goal is fewer visual breaks, not decorated ones.

---

## TYPOGRAPHY

### Font Families
- **Monospace:** `'JetBrains Mono'`, `'Fira Code'`, `'SF Mono'`, fallback `'Courier New', monospace` — used for labels, data, section headers (the "instrument readout" voice)
- **Sans-serif:** System UI stack (`'Inter', -apple-system, system-ui, sans-serif`) — used for body/descriptive text only

### Conventions
- **Section Labels:** ALL_CAPS, prefixed with `//`, color = Accent (`#e0a458`)
- **Data/Metrics:** Monospace, color `#e8ecf1` (primary) or `#7c8b9b` (secondary)
- **Status:** Binary only — `ACTIVE` (Accent) or a muted outline tag (Secondary Tint). No green/red/cyan status colors.

---

## COMPONENT SPECIFICATIONS

### Badge Specification (shields.io)
```
https://img.shields.io/badge/[LABEL]-1a2330?style=flat-square&logo=[LOGO]&logoColor=e0a458
```
- `style=flat-square` — never `plastic`, `flat`, or `for-the-badge`
- Background `1a2330` (a shade lighter than page background, reads as a quiet chip) — never the brand's own color. `e0a458` was tried as the badge fill directly and rejected: shields auto-picks white message text on it, and white-on-`#e0a458` measures ~2.2:1 contrast, well under WCAG AA
- `logoColor=e0a458` (accent) is the only accent surface on the badge — the logo icon. Message text stays shields' auto white, which lands close to `Text Primary`
- Group by category with a plain-text category caption (monospace, Text Dim) above each row rather than separating categories with extra cards

### Stat Card Specification (github-readme-stats / streak-stats)
All stat card URLs must use these overrides:
```
theme=transparent
hide_border=true
bg_color=0b0f14
title_color=e0a458
icon_color=e0a458
text_color=e8ecf1
border_color=202b36
ring_color=e0a458
```
Never use pre-built themes (`theme=radical`, `theme=dark`, etc.) — always override every color param manually so the widget matches the page instead of bringing its own palette.

### Snake Contribution Graph (Platane/snk)
Pass explicit `color_dots` and `color_snake` query params (5-step ramp from Surface to Accent, snake body in Text Primary → Accent) instead of a bundled `palette=github-dark` preset — same rationale as stat cards.

### SVG Assets
All custom SVGs must:
- Include `xmlns="http://www.w3.org/2000/svg"`, declare `viewBox`
- **NEVER** include `<style>`, `<script>`, or `class` attributes (GitHub strips them)
- **NEVER** rely on CSS animation in `<img>`-served SVGs (GitHub blocks it) — SMIL `<animate>` is fine and used sparingly (banner tagline cross-fade only)
- Use static SVG elements only: `<rect>`, `<circle>`, `<line>`, `<path>`, `<polygon>`, `<text>`, `<pattern>`
- Specify fonts explicitly per the Typography section above
- Keep files under 50KB; use `<path>` for the contour motif rather than hundreds of discrete elements

---

## SECTION ARCHITECTURE

### What's actually on the page (v2026.1)
1. **Banner** — name, rotating role tagline, contour motif (replaces the old animated signal-bar + crosshair graphic)
2. **Operator Profile** — ASCII portrait (generated from a real photo) + fetch-style bio panel
3. **Current Focus** — one slim panel, only the genuinely true "what I'm building right now" list (the old fabricated STATUS/UPTIME/MODE and SIGNAL/geo cards are gone — they said nothing real)
4. **System Pulse** — GitHub stats + streak + top languages, restyled to palette
5. **Contribution Telemetry** — snake graph, restyled to palette
6. **Capabilities** — one unified, categorized badge list (merges the old separate Runtime Stack + Infrastructure Surface + AI Systems Surface tables into a single section, since they were all answering "what do you work with")
7. **Active Systems** — pinned repos
8. **Open Channel** — contact links, same two-tone badge treatment
9. **Footer** — contour divider + profile-view counter

### Cut entirely (and why)
- **WakaTime Telemetry** — no workflow ever generated it (`.github/workflows/` only contains `snake.yml`); the section rendered from an unverified third-party endpoint. Cut rather than fixed, since it wasn't real.
- **Portfolio `curl` mock** — a fake terminal transcript pretending to be a live request. Replaced by a plain link.
- **Live Feed** — duplicated Current Focus with different formatting.
- **Infrastructure Surface** — duplicated Runtime Stack.
- Fabricated flavor stats (`NODE: India`, `UPTIME: CONTINUOUS`, `SIGNAL: ACTIVE` with fake concentric rings) — decorative claims with no real referent.

### Section Dividers
Use the contour motif (`divider.svg`) only between the major acts listed above. Otherwise, use plain vertical spacing — not another `---` rule. The old version put a `---` between almost every subsection, which is what made the page feel like stacked index cards.

---

## ASSET FILE STRUCTURE

Actual, current layout (flat — no `assets/` or `generated/` subfolders exist, keep this section honest):

```
AaditPani-RVU/
├── README.md
├── DESIGN.md                # this file
├── banner.svg
├── operator-profile.svg
├── current-focus.svg
├── divider.svg
├── 155768418.jpg            # source photo for the ASCII portrait
└── .github/
    └── workflows/
        └── snake.yml         # the only workflow that actually exists
```

---

## ACCESSIBILITY & RENDERING

- GitHub sanitizes `<style>`, `<script>`, `class` attributes with external CSS; does not strip `<div>`, `<table>`, `<img>`, `align`, `alt`, inline `id`s
- SVG animations are stripped when served via `<img src="...">` except SMIL `<animate>`, used sparingly
- All stat cards use `theme=transparent` + explicit color params (never a bundled theme)
- Every image has real `alt` text
- Palette maintains WCAG AA contrast against `#0b0f14`

---

## EVOLUTION & VERSIONING

Version tracked in the banner: `v2026.1`. Bump on any palette or structural change. If a future pass wants to reintroduce a widget that was cut above, it needs to be *real* (backed by an actual workflow/data source) before it goes back in — that was the failure mode last time.
