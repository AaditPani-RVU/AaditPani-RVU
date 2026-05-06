# DESIGN SYSTEM: AaditPani-RVU Profile

A production-grade design system for adaptive AI systems engineering identity. This document formalizes the aesthetic, component specifications, and architectural rationale.

---

## COLOR PALETTE (Strict Reference)

| Role | Value | Usage |
|---|---|---|
| Background | `#0a0a0a` | Primary page background |
| Surface | `#111111` / `#141414` | Card/panel backgrounds |
| Border | `#1e1e1e` / `#252525` | Dividers, outlines |
| Primary Accent | `#00b4d8` | Section labels, highlights, CTAs |
| Secondary Accent | `#f59e0b` | Status indicators, metrics accents |
| Text Primary | `#e2e8f0` | Body text, content |
| Text Muted | `#64748b` | Secondary text, metadata |
| Success | `#22c55e` | Active/deployed status |
| Danger | `#ef4444` | Error states (reserved) |
| Graphite | `#374151` | Neutral borders, dividers |

**Design Principle:** All colors must maintain WCAG AA contrast ratios against their backgrounds. Never use pure white (`#ffffff`) for text — use `#e2e8f0`.

---

## TYPOGRAPHY

### Font Families
- **Monospace:** `'JetBrains Mono'`, `'Fira Code'`, `'SF Mono'`, fallback `'Courier New', monospace`
- **Sans-serif:** System UI stack (for headings when needed)

### Sizes & Weights
| Context | Size | Weight | Line-Height |
|---|---|---|---|
| Banner Title | 48px | 700 | 1.0 |
| Banner Subtitle | 16px | 400 | 1.2 |
| Section Label | 14px | 600 | 1.4 |
| Body Text | 13px | 400 | 1.5 |
| Small/Metadata | 11px | 400 | 1.4 |
| Badge Label | 12px | 600 | 1.0 |

### Conventions
- **Section Labels:** ALL_CAPS or `SCREAMING_SNAKE_CASE`, prefixed with `//`, color `#00b4d8`
- **Data/Metrics:** Monospace, large, standalone, color `#e2e8f0` or `#64748b`
- **Status Keywords:** `ACTIVE`, `BUILDING`, `RESEARCH`, `DEPLOYED`, `MONITORING` — always uppercase, always badged

---

## COMPONENT SPECIFICATIONS

### Badge Specification (shields.io)
All badges must conform to:
```
https://img.shields.io/badge/[LABEL]-[COLOR]?style=flat-square&logo=[LOGO]&logoColor=white
```

**Requirements:**
- `style=flat-square` — never `plastic`, `flat`, or `for-the-badge`
- `logoColor=white` — unless brand color is naturally light
- Label: `SCREAMING_SNAKE_CASE` or short slug (max 20 chars)
- Color: Use palette values (hex) or brand-official hex
- No badge wider than 160px

**Example:**
```
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
```

### Stat Card Specification (github-readme-stats)
All stat cards must use these parameters:
```
theme=transparent
hide_border=true
bg_color=0d0d0d
title_color=00b4d8
text_color=e2e8f0
icon_color=f59e0b
```

Never use pre-built themes like `theme=radical`, `theme=dark`. Always override all color parameters manually.

**Example:**
```
https://github-readme-stats.vercel.app/api?username=AaditPani-RVU&theme=transparent&hide_border=true&bg_color=0d0d0d&title_color=00b4d8&text_color=e2e8f0&icon_color=f59e0b
```

### SVG Assets
All custom SVGs must:
- Include `xmlns="http://www.w3.org/2000/svg"`
- Declare `viewBox` and `preserveAspectRatio="xMidYMid meet"`
- Use `<pattern>` IDs for reusable elements (grids, scanlines)
- **NEVER** include `<style>`, `<script>`, or `class` attributes (GitHub strips them)
- **NEVER** use CSS animations (GitHub blocks them in `<img>` tags)
- Use static SVG elements only: `<rect>`, `<circle>`, `<line>`, `<polygon>`, `<text>`, `<pattern>`
- Specify fonts explicitly: `font-family="'JetBrains Mono', 'Courier New', monospace"`

---

## SECTION ARCHITECTURE

### Section Label Naming Convention
Format: `// NOUN_NOUN`
- Always two words
- Always uppercase
- Always underscore-joined
- Always prefixed with `//`
- Always in muted cyan `#00b4d8`

Examples:
- `// COMMAND_CENTER` — system status panels
- `// SYSTEM_PULSE` — GitHub stats
- `// RUNTIME_STACK` — tech badges
- `// AI_SYSTEMS_SURFACE` — domain competencies
- `// LIVE_FEED` — active work threads

### Section Dividers
Between major sections:
```markdown
---
```
(Blank line before, blank line after)

Or use custom SVG:
```html
<img src="./assets/divider.svg" alt="divider" width="100%" />
```

### Layout Grid
- **Maximum content width:** 860px (for centered elements)
- **Multi-column layouts:** Always use HTML `<table>` with `align="center"` on wrapper `<div>`
- **Badge rows:** Left-aligned, max 3 badges per line on mobile
- **Spacing:** `&nbsp;` between badge groups, blank lines between sections

---

## COMPONENT EXAMPLES

### Command Center Panel
Three-column layout using `<table>`:
```html
<table>
  <tr>
    <td><pre>LABEL
TEXT
MORE</pre></td>
    <td><pre>LABEL
TEXT
MORE</pre></td>
    <td><pre>LABEL
TEXT
MORE</pre></td>
  </tr>
</table>
```

### Badge Row
```html
![Label1](https://img.shields.io/badge/...) ![Label2](https://img.shields.io/badge/...)
```
Group with blank line between categories.

### Status Table
Markdown table with status badges inline:
```markdown
| DOMAIN | CAPABILITY | STATUS |
|:---|:---|:---:|
| Name | Description | ![ACTIVE](https://img.shields.io/badge/ACTIVE-22c55e?style=flat-square) |
```

### Multi-Column Card Layout
```html
<div align="center">
<table>
  <tr>
    <td align="center">
      <a href="link">
        <img src="card-url" alt="description" />
      </a>
    </td>
    <td align="center">
      <a href="link">
        <img src="card-url" alt="description" />
      </a>
    </td>
  </tr>
</table>
</div>
```

---

## ASSET FILE STRUCTURE

```
AaditPani-RVU/
├── README.md                    # Main profile
├── DESIGN.md                    # This file
├── assets/
│   ├── banner.svg              # 1200×300, dot-grid + scanlines
│   ├── divider.svg             # 1200×20, center diamond
│   └── terminal-prompt.svg     # 600×60, decorative terminal
├── .github/
│   └── workflows/
│       ├── snake.yml           # Contribution snake (weekly)
│       ├── waka-readme.yml     # WakaTime stats (daily)
│       └── metrics.yml         # GitHub metrics (weekly)
└── generated/
    ├── snake-dark.svg          # Auto-generated by workflow
    ├── snake-light.svg         # Auto-generated by workflow
    └── metrics.svg             # Auto-generated by workflow
```

---

## ACCESSIBILITY & RENDERING

### GitHub Markdown Rendering
- GitHub sanitizes: `<style>`, `<script>`, `class` attributes with external CSS
- GitHub does NOT strip: `<div>`, `<table>`, `<img>`, `align`, `alt` attributes, inline `id`s
- SVG animations are stripped when served via `<img src="...">`
- Use `alt` attributes on all images for screen readers

### Color Mode Compatibility
- Use `transparent` backgrounds on all stat cards
- Test in both GitHub light and dark modes
- Never rely on CSS media queries (GitHub strips them)

### Image Optimization
- Keep SVG files under 50KB
- Use patterns for repeating elements (dots, scanlines)
- Use `viewBox` instead of fixed dimensions for responsiveness
- Always include `alt` text

---

## WORKFLOW AUTOMATION

### Snake Contribution Graph
**Trigger:** Weekly Sunday midnight + push to main  
**Output:** `generated/snake-dark.svg`, `generated/snake-light.svg`  
**Tool:** `Platane/snk@v3`

### WakaTime Telemetry
**Trigger:** Daily 18:30 UTC (midnight IST)  
**Secrets Required:** `WAKATIME_API_KEY`, `GH_TOKEN`  
**Tool:** `anmol098/waka-readme-stats@master`

### GitHub Metrics
**Trigger:** Weekly Monday 04:00 UTC  
**Secrets Required:** `METRICS_TOKEN`  
**Tool:** `lowlevel-studios/github-metrics@v2`

---

## QUALITY CHECKLIST

Before deploying any changes:

- [ ] All GitHub Stats URLs use correct username `AaditPani-RVU`
- [ ] All SVGs have `xmlns="http://www.w3.org/2000/svg"`
- [ ] No `<style>`, `<script>`, or `class` in SVG files
- [ ] No CSS animations in SVG (GitHub blocks them in `<img>` tags)
- [ ] All stat card URLs include `theme=transparent`
- [ ] All badges use `style=flat-square`
- [ ] Color palette strictly adhered to (no off-brand colors)
- [ ] Every section has `// NOUN_NOUN` label in `#00b4d8`
- [ ] All images have `alt` attributes
- [ ] README renders coherently in dark mode (primary) and light mode
- [ ] Links are functional and point to correct resources
- [ ] Workflows have correct triggers and secrets configured
- [ ] SVG files display correctly in both light and dark GitHub themes

---

## DESIGN RATIONALE

### Aesthetic Philosophy
"AI systems dashboard / telemetry control center / autonomous systems interface"

The design language mirrors observability platforms and embedded systems dashboards, not gaming or creative tools. This communicates:
- **Precision:** Every element is intentional, no decoration
- **Depth:** Layered information density (banner → stats → capabilities → projects)
- **Credibility:** Professional systems engineering, not personality
- **Autonomy:** The dashboard metaphor suggests unattended operation

### Section Order Rationale
1. **Banner** — Immediate brand/identity impact
2. **Command Center** — Establish current state and focus
3. **System Pulse** — Prove activity with GitHub stats
4. **Contribution Telemetry** — Visual proof of consistent work
5. **Runtime Stack** — Technical credibility across multiple domains
6. **Active Deployments** — Demonstrate shipped projects
7. **AI Systems Surface** — Deep domain expertise visualization
8. **WakaTime Telemetry** — Transparency about work habits
9. **Live Feed** — Dynamic feeling, ongoing momentum
10. **Infrastructure Surface** — Operational breadth
11. **Open Channel** — Call-to-action for collaboration
12. **Footer** — Aesthetic close

This progression moves from identity → proof of work → capability → availability.

---

## EVOLUTION & VERSIONING

This profile is designed for evolution:
- **Color palette:** Can be adjusted globally by updating 4-5 CSS-like variable definitions
- **Section content:** Can be updated without restructuring (content is data-driven)
- **Workflows:** Can be replaced or extended (modular GitHub Actions)
- **SVG assets:** Can be regenerated or replaced (maintain viewBox/aspect ratios)

Version tracked in banner: `v2025.1` → Update on major changes.

---

