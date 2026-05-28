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
Claim → Evidence (exact source quote) → Analysis → Confidence.

## 3. Synthesis / Phase Model (if applicable)
Periodization or model with visual representation.

## 4. Policy Implications / Significance
What do findings mean? For whom? What would change them?

## 5. Limitations
Data limits, methodological caveats, direction of potential bias.

## 6. Future Research
Unanswered questions, what new data would help, out-of-scope items.

## 7. References
Every source cited with exact quote references. Cross-reference to sources_index.md.
```

## Output Specification

Reports are generated as **complete HTML pages** within the `project/web/` site. Each finding uses this structure within the report:

```html
<div class="finding" data-confidence="HIGH|MEDIUM|LOW" data-source="SOURCE-ID">
  <h3>Finding Title</h3>
  <div class="claim">The core claim, stated directly.</div>
  <div class="evidence">
    "Exact quoted text from the source material."
    <em>— Issuing Body, Date</em>
  </div>
  <div class="analysis">
    Interpretation, cross-references to other findings, caveats.
  </div>
  <div><span class="confidence-badge confidence-HIGH">HIGH</span></div>
</div>
```

### Attribute Rules

- **`data-confidence`**: Required on every finding div. Values: `HIGH` (multiple independent sources), `MEDIUM` (single source or indirect), `LOW` (extrapolation or limited evidence).
- **`data-source`**: Required on every finding div. Space-separated list of source IDs from `sources_index.md`. Do NOT retrofit after writing — specify citations when drafting each finding.
- **Confidence badge**: Each finding must end with a visible `<span class="confidence-badge">` matching its `data-confidence`.
- These attributes enable the bilingual parity verification script (`final_verify.py`) to detect missing citations.

### Structural Rules

- No inline styles (`style="..."`) — use semantic classes only
- No scripts (`<script>`)
- Every finding needs a unique `data-source` (can share IDs across findings)
- The `<h3>` title should match across EN/ZH versions for structural parity

## Writing Principles

- **Claim → Evidence → Analysis → Confidence**: every finding follows this sequence
- **Plain language first**: executive summary accessible to non-specialists
- **Visual anchor**: key finding gets a table/chart/diagram within 3 paragraphs
- **Cross-reference findings**: synthesize across domains, don't silo
- **Source IDs in findings enable automated verification**: without them, parity and grounding cannot be checked at scale

## Visuals Matching Findings

| Finding Type | Best Visual | Location |
|---|---|---|
| Overview / geography | SVG map | `project/web/images/` |
| Time-series trends | Line chart | `project/web/images/` |
| Correlations | Heatmap / scatter | `project/web/images/` |
| Event impacts | Before/after bars | `project/web/images/` |
| Comparisons | Grouped bars / radar | `project/web/images/` |
| Phases | Colored cards / timeline | Inline or SVG |

## Cross-Phase Integration

**Important:** The `data-source` attributes written here feed directly into the Phase 6 encoding verification and Phase 7 bilingual parity checks. If a finding lacks `data-source`, it will appear as a gap in the automated verification script (`final_verify.py`). Write citations first — do not plan to add them later.
