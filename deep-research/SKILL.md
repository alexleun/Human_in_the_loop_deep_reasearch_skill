---
name: deep-research
description: Deep research workflow with human guidance. Topic-agnostic. For detailed phase instructions, load the corresponding sub-file from this directory.
license: MIT
metadata:
  version: "4.0"
  files:
    - "01-explore.md"
    - "02-propose.md"
    - "03-collect.md"
    - "04-analyze.md"
    - "05-report.md"
    - "06-web-output.md"
    - "07-review.md"
    - "08-iterate.md"
    - "09-archive.md"
    - "state-management.md"
---

# Deep Research Skill

A modular skill for structured deep research in collaboration with a human. Works on **any topic**.

---

## Research Lifecycle

```
  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
  │  EXPLORE  │───▶│ PROPOSE  │───▶│  APPLY   │───▶│  REVIEW  │
  │ (think)   │    │ (design) │    │ (do)     │    │ (audit)  │
  └──────────┘    └──────────┘    └──────────┘    └──────────┘
       │                              │                 │
       │   ┌──────────────────────────┘                 │
       │   │   ┌────────────────────────────────────────┘
       ▼   ▼   ▼
    ┌──────────────────────────────────────────────────────┐
    │                    ITERATE                            │
    └──────────────────────────────────────────────────────┘
```

---

## Core Methodology Principles

These apply across ALL phases. Read them first.

### 1. Objective-Driven, Not Role-Driven

State what needs to be done. Do not assign personas or roles to the LLM. Role constraints consume attention tokens and cause "role capture" — the LLM performs confidence instead of actual reasoning. Short, direct instructions outperform elaborate personas.

Good: "Extract the three key metrics from this source and cite exact text."
Bad: "You are a senior research analyst at a top-tier think tank..."

### 2. Grounding (Anti-Hallucination)

Every factual output must cite **exact quoted text** from the source material. Paraphrasing introduces hallucination risk.

- Data extraction: include `"exact_quote": "the verbatim text"` in the output
- Source citation: include the specific sentence or paragraph, not just the URL
- If the source lacks evidence for a claim, mark it `[Data Gap]` — never fill in from memory

### 3. Code-First Analysis

For any quantitative task: **output code, not numbers.** The LLM writes Python that the human runs locally. Results come from actual execution, not LLM memory.

- Good: "Write a Pandas script to compute the correlation. Here is the CSV."
- Bad: "The correlation coefficient is approximately 0.87."

### 4. Binary Choice for Output Blocks

When the output spec offers multiple formats (JSON vs Python, HTML vs Markdown), the LLM must pick **one** per block. Mixing formats in the same block causes decoding errors.

### 5. Citation-First (Not Retrofitted)

Source citations must be embedded from Phase 2, not added in Phase 7 review. Every change proposal must specify the `data-source` IDs and confidence levels that findings will carry. This prevents the retrofitting pain experienced when adding `data-source` attributes to every claim block after all reports were written — a process that required 5+ scripts and multiple parity fixes.

### 6. Encoding Awareness (for CJK Content)

When generating Chinese/Japanese/Korean content on Windows systems, PowerShell encoding behavior corrupts files:
- `>` and `|` operators use the system code page, not UTF-8
- U+FFFD replacement characters appear when UTF-8 bytes are decoded as Windows-1252
- Fix: Always use Python `open(path, "w", encoding="utf-8")` or .NET `[System.IO.File]::WriteAllText()`
- Verify: After writing a ZH file, re-read it and check for `\uFFFD` characters

### 7. Bilateral Parity Verification

"Bilingual parity" is not a one-time goal — it requires automated verification at each phase boundary. For every EN file, the corresponding ZH file must pass:
- Same count of `<div class="finding">` blocks
- Same count of `data-source` attributes
- Same count of `data-confidence` attributes
- No `\uFFFD` encoding errors
- Consistent nav labels across all files in the language

### 8. Review Manifest (Optional, Phase-Dependent)

When the task is complex or the human needs a checkpoint:
- Use a short text summary (3-5 bullets), not a YAML block
- Include: what was done, how many sources/claims were processed, what gaps remain
- Skip when the human is actively guiding — the conversation log serves as the manifest

**Experience note:** In practice, the Review Manifest was never used during the HK research project. The human-in-the-loop workflow naturally provides checkpointing through conversation. Reserve manifests for fully autonomous sub-agent tasks.

---

## Phase Router

Load the sub-file matching your current task:

| Current Task | Load This File |
|---|---|
| Scoping topic, exploring ideas, discussing with human | `01-explore.md` |
| Creating change proposal, design, specs, tasks | `02-propose.md` |
| Collecting sources, data pipeline, building knowledge base | `03-collect.md` |
| Running analysis, writing notebooks, generating figures | `04-analyze.md` |
| Writing research document | `05-report.md` |
| Generating bilingual website, HTML pages, SVG map | `06-web-output.md` |
| Auditing for methodological weaknesses | `07-review.md` |
| Developing new point of view, reframing | `08-iterate.md` |
| Finalizing deliverables, archiving the change | `09-archive.md` |
| Switching machines, resuming interrupted work | `state-management.md` |

---

## Collaboration Model

| Task | LLM does | Human does |
|---|---|---|
| Data collection | Fetches, extracts, validates | Provides access, flags bias |
| Analysis | Writes code, runs computations | Interprets, redirects |
| Report writing | Drafts document, generates web pages | Reviews, adds nuance |
| Design | Proposes methodology, flags tradeoffs | Approves/rejects, sets priorities |
| Audit | Checks methodology, flags issues | Identifies non-obvious weaknesses |
| Decisions | Presents options with recommendations | **Makes the call** |

**Key principle:** LLM proposes, human disposes.

---

## Guardrails

- **Propose before implementing** — no data collection without a proposal
- **Log every source** — local copy in `knowledge-base/sources/` for fact-checking
- **Never silently fix** — surface methodological issues before fixing
- **Bilingual parity** — both languages must have same content and quality; verify with automated script, not manually
- **Verify encoding after generation** — especially CJK content on Windows; check for U+FFFD before considering a file complete
- **Don't delete old versions** — use `_v1`, `_v2` suffixes
- **Flag uncertainty** — distinguish facts, claims, hypotheses
- **Verify CSVs against authoritative sources** — do not assume web-scraped or LLM-generated data is correct; cross-check key numbers
