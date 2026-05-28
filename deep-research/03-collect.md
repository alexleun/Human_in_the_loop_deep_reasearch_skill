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
- Store raw data (unaltered) in `project/data/raw/`
- Store processed data in `project/data/` with provenance README
- **Code-first**: write Python scripts for all transforms, not manual steps

## 3c. Data Verification (Critical — Learned from Practice)

Web-scraped and LLM-generated CSV data frequently contains errors. After initial collection:

1. **Extract key inflection points**: For every CSV, identify 3-5 critical data points (e.g., most recent year, historical peak, trough, key ratio)
2. **Cross-verify against authoritative sources**: Fetch each key point from the original gov't/agency publication and confirm the CSV value matches
3. **Create a correction log**: For each discrepancy found, document in `project/data/verification_log.md`:
   ```
   ## CSV: population.csv
   - **Error**: 2025 end-year population was 7,503,000 (wrong) vs 7,510,800 (correct)
   - **Source**: C&SD Year-end Population for 2025 (2026-02-12)
   - **Fix**: Updated cell B26 in population.csv
   - **Propagation**: Re-ran generate_all_figures.py after fix
   ```
4. **Propagate corrections**: A single CSV fix may affect multiple figures and report claims. After fixing a CSV:
   - Re-run all figure generation scripts
   - Re-check any report text that references the corrected value
   - Flag "pre-correction" and "post-correction" values in the methodology page
5. **Track figure impact**: If figure axes or titles reference specific values, update them too

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
