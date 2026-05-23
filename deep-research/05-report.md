# Phase 5: Apply — Report Writing

**Purpose:** Turn findings into a well-structured research document.

## Report Template

```
# Title: <Topic> — <Subtitle>

## Executive Summary
Top findings in plain language (5-7 bullets max). One-sentence takeaway.

## Methodology
Data sources, analytical approach, limitations, reproducibility instructions.

## 1. Background & Context
Why this matters, historical context, key concepts.

## 2. Findings (one section per major finding)
Claim → Evidence (exact source quote) → Alternative explanations → Confidence.

## 3. Synthesis / Phase Model (if applicable)
Periodization or model with visual representation.

## 4. Policy Implications / Significance
What do findings mean? For whom? What would change them?

## 5. Limitations
Data limits, methodological caveats, direction of potential bias.

## 6. Future Research
Unanswered questions, what new data would help, out-of-scope items.

## References
Every source cited with exact quote references. Cross-reference to sources_index.md.
```

## Output Specification

Each section of the report is written as an **HTML fragment** using semantic tags only:

```html
<section class="finding">
  <h2>Finding Title</h2>
  <p class="claim">The core claim, stated directly.</p>
  <blockquote class="evidence" data-source="source-id">
    "Exact quoted text from the source material."
    <cite>— Issuing Body, Date</cite>
  </blockquote>
  <p class="confidence" data-level="high">Supported by N independent sources.</p>
</section>
```

Rules:
- No inline styles (`style="..."`) — use semantic classes only
- No scripts (`<script>`)
- Every `<blockquote>` includes a `<cite>` with source
- Every `<p class="confidence">` has a `data-level` attribute: `high`, `medium`, `low`, or `contested`

## Writing Principles

- **Claim → Evidence → Uncertainty**: every claim links to its exact source quote
- **Plain language first**: executive summary accessible to non-specialists
- **Visual anchor**: key finding gets a table/chart/diagram within 3 paragraphs

## Visuals Matching Findings

| Finding Type | Best Visual | Location |
|---|---|---|
| Overview / geography | SVG map | `output/<topic>-map.svg` |
| Time-series trends | Line chart | `analysis/figures/` |
| Correlations | Heatmap / scatter | `analysis/figures/` |
| Event impacts | Before/after bars | `analysis/figures/` |
| Comparisons | Grouped bars / radar | `analysis/figures/` |
| Phases | Colored cards / timeline | Inline or SVG |

## Review Manifest

```yaml
# STEP_REVIEW_MANIFEST
section: "Findings — [section title]"
claims_made: N
sources_cited: N
data_gaps:
  - "Any claims where evidence was weaker than desired"
next_action: "Write next section or compile into final document"
```
