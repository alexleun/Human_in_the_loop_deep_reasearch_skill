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

### 5. Review Manifest

After each step, produce a structured review checkpoint:

```yaml
# STEP_REVIEW_MANIFEST
summary: "One-line summary of what this step accomplished"
evidence_chain:
  - "Claim A -> source reference (exact quote)"
  - "Claim B -> source reference"
data_gaps:
  - "What the source material could not confirm"
next_action: "Suggested next step"
```

The human reviews this manifest (not the full output) to decide whether to continue.

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
- **Bilingual parity** — both languages must have same content and quality
- **Commit + push after every task** — enables machine switching
- **Don't delete old versions** — use `_v1`, `_v2` suffixes
- **Flag uncertainty** — distinguish facts, claims, hypotheses
