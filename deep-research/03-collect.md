# Phase 3: Apply — Data Collection

**Purpose:** Gather raw material — documents, datasets, primary sources.

## 3a. Source Collection

**Source hierarchy:** Government/regulatory > Statistical agencies > Academic papers > Think tank reports > Industry reports > News (context only)

**Grounding requirement — every source entry must include:**
- Title, author/issuing body, date, URL/DOI
- **Exact quoted text** of the specific passage used (not a summary)
- Confidence level: HIGH (multiple sources agree) / MEDIUM (single source) / LOW (indirect)

**Preserve for fact-checking:**
- Save local copy to `knowledge-base/sources/` named `YYYY-MM-DD-description.{pdf,html,md}`
- Maintain `knowledge-base/sources/sources_index.md` with all metadata
- For datasets: save download with date stamp, note any transformations

## 3b. Data Pipeline (if quantitative)

- Fetch from authoritative APIs; validate temporal coverage, outliers, schema
- Store raw data (unaltered) in `analysis/data/raw/`
- Store processed data in `analysis/data/` with provenance README
- **Code-first**: write Python scripts for all transforms, not manual steps

## 3c. Knowledge Base

- Populate `knowledge-base/<dimension>/` per topic dimension
- Each entry links to source and includes exact quoted evidence
- Distinguish: **facts** (multi-source confirmed), **claims** (single source), **hypotheses** (analysis), **contested** (sources disagree)
- Update `knowledge-base/timeline.md` with new events

## Review Manifest

After each collection batch, produce:

```yaml
# STEP_REVIEW_MANIFEST
summary: "Collected N sources on [subtopic]"
evidence_chain:
  - "[Claim] -> [Source title], [exact quote]"
  - "[Claim] -> [Source title], [exact quote]"
data_gaps:
  - "What could not be found or confirmed"
next_action: "Collect sources for next subtopic or move to analysis"
```
