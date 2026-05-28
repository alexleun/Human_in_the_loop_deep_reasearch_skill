# Phase 6: Apply — Web Output Generation

**Purpose:** Produce a bilingual website presenting the research professionally.

## Site Architecture (Actual)

Based on the HK research project implementation, the effective structure is:

```
project/web/
├── index.html                  ← Landing page (English, with EN/繁 switcher)
├── final-report.html           ← Comprehensive final report
├── methodology.html            ← Data pipeline, correction log, confidence framework
├── synthesis.html              ← Integrated cross-domain analysis
├── scenario-analysis.html      ← Scenario projections
├── hk-sg-benchmark.html        ← Comparison tables
├── systems-integration.html    ← Causal loop diagrams
├── zh/                         ← Traditional Chinese mirror
│   ├── index.html
│   ├── final-report.html
│   ├── methodology.html
│   ├── synthesis.html
│   ├── scenario-analysis.html
│   ├── hk-sg-benchmark.html
│   └── systems-integration.html
├── css/
│   └── style.css               ← Shared stylesheet
└── images/                    ← Generated PNG figures
    ├── figure-01.png
    └── ...
```

**Key differences from original `output/` design:**
- ZH pages live in a `zh/` subdirectory, not separate `*-zh.html` files
- Image paths in ZH pages use `../images/` prefix
- Nav links within `zh/` stay within the language (no `../`), language switcher alone crosses the boundary

## Design System

### Color Palette

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
- **Highlight boxes**: colored left-border (blue=finding, red=critical, gold=key)

## Navigation Pattern

All pages share a consistent nav structure with EN/繁 language switcher:

```html
<nav class="top-nav">
  <span class="nav-brand">Project Name</span>
  <span class="nav-sep">|</span>
  <span class="nav-group">
    <a href="index.html">Home</a>
    <a href="final-report.html">Final Report</a>
    <a href="synthesis.html">Synthesis</a>
    <a href="scenario-analysis.html">Scenarios</a>
    <a href="hk-sg-benchmark.html">HK-SG Benchmark</a>
    <a href="systems-integration.html">Systems Map</a>
  </span>
  <span style="margin-left:auto;">
    <a href="../index.html">EN</a> / <strong>繁</strong>
  </span>
</nav>
```

**Critical nav rules from experience:**
- EN nav: all `href` paths are flat (`href="final-report.html"`)
- ZH nav: all `href` paths within the same language are flat (`href="final-report.html"`)
- ZH nav: ONLY the language switcher uses `../` to cross into English (`href="../index.html"`)
- All 36 pages (18 EN + 18 ZH) must be checked for nav consistency

## Landing Page Structure

```
NAV: sticky bar
HERO: title + subtitle + metadata
STATS GRID: key metrics in colored cards
DOMAIN CARDS: each linking to its domain report
FIGURES GALLERY: 10+ charts with captions
FOOTER: source citation, disclaimer, last updated
```

## Report Page Structure

Each report uses this consistent structure:

```html
<div class="report-page">
  <h1>Title</h1>
  <h2>Section</h2>
  <div class="finding" data-confidence="HIGH|MEDIUM|LOW" data-source="SOURCE-ID">
    <h3>Finding Title</h3>
    <div class="claim">Core claim stated directly.</div>
    <div class="evidence">
      "Exact quoted text from source."
    </div>
    <div class="analysis">
      Interpretation and cross-referencing.
    </div>
    <div><span class="confidence-badge confidence-HIGH">HIGH</span></div>
  </div>
</div>
```

Rules:
- Every `data-confidence` maps to a confidence badge: HIGH / MEDIUM / LOW
- Every `data-source` references IDs from `sources_index.md`
- No inline styles — use semantic CSS classes only
- No `<script>` tags

## Bilingual Content Strategy

| Element | Approach |
|---|---|
| Page structure | Same HTML template for both languages |
| Navigation | Translated labels, verified consistent across ALL pages |
| Content | Full translation, not summaries |
| Figures | Same for both (language-neutral) |
| CSS | Same system; Chinese pages use serif fonts |
| Language switch | EN → links to `../zh/`, ZH → links to `../` |
| Image paths | ZH pages use `../images/` prefix |
| Encoding | UTF-8 without BOM; verify no U+FFFD after generation |

## Pre-Deployment Verification

Before considering the web output complete, run an automated check:

```python
# final_verify.py — checks all 5 high-level report pairs
import re, os

pairs = [
    ("final-report.html", "zh/final-report.html"),
    ("synthesis.html", "zh/synthesis.html"),
    ("scenario-analysis.html", "zh/scenario-analysis.html"),
    ("hk-sg-benchmark.html", "zh/hk-sg-benchmark.html"),
    ("systems-integration.html", "zh/systems-integration.html"),
]

base = "project/web"
all_ok = True
for en_rel, zh_rel in pairs:
    en_text = open(os.path.join(base, en_rel), encoding="utf-8").read()
    zh_text = open(os.path.join(base, zh_rel), encoding="utf-8").read()
    en_ds = en_text.count('data-source="')
    zh_ds = zh_text.count('data-source="')
    en_conf = en_text.count('data-confidence="')
    zh_conf = zh_text.count('data-confidence="')
    en_fffd = en_text.count('\ufffd')
    zh_fffd = zh_text.count('\ufffd')
    ok = en_ds == zh_ds and en_conf == zh_conf and en_fffd == 0 and zh_fffd == 0
    print(f"{en_rel}: EN ds={en_ds} ZH ds={zh_ds} | EN conf={en_conf} ZH conf={zh_conf} | EN encoding={'OK' if en_fffd==0 else 'CORRUPT'} ZH encoding={'OK' if zh_fffd==0 else 'CORRUPT'} | {'✓' if ok else '✗'}")
    if not ok: all_ok = False

print(f"\nAll matched: {all_ok}")
```

Also verify:
- All EN nav links: check every `<a href="..."` in nav groups resolves to an existing file
- All ZH nav links: same check within zh/ subdirectory
- All ZH image paths: check for `src="../images/"` (not `src="images/"`)

## Encoding Strategy (for CJK Content on Windows)

Windows PowerShell's `>` and `|` operators encode text using the system code page (Windows-1252 for en-US), NOT UTF-8. This corrupts Chinese characters:

**BAD (causes U+FFFD):**
```powershell
$html | Out-File zh/file.html
$html > zh/file.html
```

**GOOD:**
```powershell
[System.IO.File]::WriteAllText("zh/file.html", $html, [System.Text.UTF8Encoding]::new($false))
```

Or write via Python from PowerShell:
```powershell
python -c "open('zh/file.html','w',encoding='utf-8').write(open('/dev/stdin').read())"
```

**Post-generation check:** re-read every ZH file and search for character U+FFFD (`\ufffd`). Any occurrence means the file is permanently corrupted and must be regenerated — patching is unreliable.

## SVG Map (for geographic topics)

- Bilingual title (English + Chinese)
- Colored regions, city labels, legend
- Responsive viewBox, defs for gradients/shadows
