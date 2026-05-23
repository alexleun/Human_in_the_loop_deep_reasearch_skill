# Phase 6: Apply — Web Output Generation

**Purpose:** Produce a bilingual website presenting the research professionally.

## Site Architecture

```
output/
├── index.html             ← Auto-redirect to language version
├── index-en.html          ← Landing page (English)
├── index-zh.html          ← Landing page (Traditional Chinese)
├── paper-en.html          ← Full research paper
├── paper-zh.html
├── analysis-en.html       ← Analysis details
├── analysis-zh.html
├── data-en.html           ← Data documentation
├── data-zh.html
├── notebooks-en.html      ← Notebook index
├── notebooks-zh.html
├── <topic>-map.svg        ← SVG conceptual diagram
├── figures/               ← Generated charts
├── data/                  ← Exported datasets (CSV, parquet)
└── web/
    ├── findings.html      ← Visual summary (optional)
    ├── methodology.html   ← Methodology deep-dive
    └── executive-briefing.md
```

## Design System

```css
:root {
  --primary: #1a365d;       /* Nav, headers */
  --primary-light: #2b6cb0; /* Links, accents */
  --accent: #c53030;        /* Warnings */
  --gold: #d69e2e;          /* Highlights */
  --bg: #fafafa;            /* Background */
  --text: #2d3748;
  --text-light: #718096;
}
```

- **English UI**: sans-serif (`Inter`, `Segoe UI`)
- **Chinese text**: serif (`Noto Serif TC`, `Source Han Serif TC`, `PMingLiU`)
- **Paper body**: serif (`Georgia`, `Times New Roman`)
- **Cards**: white, rounded, shadow, hover lift
- **Phase cards**: colored gradients (blue → red → green)
- **Highlight boxes**: colored left-border (blue=finding, red=critical, gold=key)

## Landing Page Structure

```
NAV: sticky bar — [Logo] Home | Paper | Analysis | Data     EN | 中文
HERO: title + subtitle + metadata
STATS GRID: key metrics in colored cards
SECTION GRID: cards linking to paper, analysis, data, notebooks
PHASE GRID (if applicable): 3 colored phase cards
FIGURES GALLERY: images with captions
FOOTER: source citation, disclaimer, last updated
```

## Redirect Page Pattern

```html
<!DOCTYPE html><html lang="en"><head>
<meta http-equiv="refresh" content="0; url=index-en.html">
</head><body>
<p>Redirect to <a href="index-en.html">English</a> or <a href="index-zh.html">中文</a>
</body></html>
```

## Output Specification

Pages are generated as **complete, valid HTML5 documents**. Each page:

1. Has a `<nav>` with identical structure across all pages
2. Uses only semantic class names (no inline styles)
3. Uses `lang="en"` or `lang="zh-Hant"` correctly
4. Links to the other language version via the language switcher
5. Embeds figures as `<img src="../figures/..." loading="lazy">`

## Bilingual Content Strategy

| Element | Approach |
|---|---|
| Page structure | Same HTML for both languages |
| Navigation | Translated labels |
| Content | Full translation, not summaries |
| Figures | Same for both (language-neutral) |
| CSS | Same system; Chinese pages use serif fonts |
| Language switch | Links to same section in other language |

## SVG Map (for geographic topics)

- Bilingual title (English + Chinese)
- Colored regions, city labels, legend
- Responsive viewBox, defs for gradients/shadows

## Review Manifest

```yaml
# STEP_REVIEW_MANIFEST
pages_generated:
  - "index-en.html"
  - "index-zh.html"
  - "paper-en.html"
nav_links_verified: true
bilingual_links_verified: true
next_action: "Human review of layout and translations"
```
