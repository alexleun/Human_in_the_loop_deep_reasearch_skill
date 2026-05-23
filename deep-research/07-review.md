# Phase 7: Review — Weak Point Audit

**Purpose:** Critically examine the research for methodological flaws, weak evidence, unstated assumptions.

## Audit Checklist

For each dimension of the research, check:

**Data & Sources**
- [ ] Is every claim grounded in exact source quotes (not paraphrases)?
- [ ] Are all sources cached in `knowledge-base/sources/` for verification?
- [ ] Are there single-source claims presented as established facts?
- [ ] Are sources from the correct hierarchy level? (gov't > academic > news)

**Analysis**
- [ ] Is any conclusion based on LLM-memorized numbers rather than computed output?
- [ ] Were statistical corrections applied where needed?
- [ ] Are there data sparsity issues that limit confidence?
- [ ] Are causal claims appropriately cautious?

**Methodology**
- [ ] Are results sensitive to arbitrary parameter choices?
- [ ] Are alternative explanations discussed?
- [ ] Are limitations disclosed with direction of bias?
- [ ] Do conclusions outrun what the data supports?

**Output**
- [ ] Is the web output consistent across languages?
- [ ] Are all navigation links functional?
- [ ] Are all source citations present and correctly formatted?

## Review Manifest

Produce one manifest per audit pass:

```yaml
# STEP_REVIEW_MANIFEST
audit_focus: "Sources & Grounding"
issues_found:
  - severity: "high"
    description: "Claim X has no source quote — only a paraphrase"
    location: "paper-en.html section 2.1"
  - severity: "medium"
    description: "Source Y not cached in knowledge-base/sources/"
    location: "sources_index.md"
passed_checks: ["All conclusions appropriately cautious", "Bilingual parity verified"]
next_action: "Fix high-severity issues before proceeding"
```

**If significant issues found → propose a new change to fix them.**
