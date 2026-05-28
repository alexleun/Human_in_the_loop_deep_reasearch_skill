# Phase 2: Propose — Research Design

**Who drives:** LLM (writes artifacts), Human (approves/redirects)

**Purpose:** Formalize the research plan as an OpenSpec change.

## Artifacts

Created via `/opsx-propose`:
- `proposal.md` — Research questions, scope, non-goals
- `design.md` — Methodology, data sources, analytical approach, web output plan
- `tasks.md` — Implementation steps broken into phases (~2hr per task)
- `specs/<capability>/spec.md` — Detailed requirements per capability

## Key Decisions to Capture

- Which data sources are authoritative vs secondary?
- How will sources be cached for fact-checking?
- What analytical methods fit the data type?
- What output languages are needed?
- What visual assets are needed? (maps, charts, diagrams)
- What would cause conclusions to change?
- **What data-source IDs will findings carry?** Specify in the proposal, not retrofitted in Phase 7. Each finding should be pre-assigned a source ID so that `data-source` attributes are written during Phase 5, not added after.
- **What confidence levels apply?** Define the threshold for HIGH/MEDIUM/LOW per dimension — prevents inconsistency across reports.
- **What verification points are needed?** For quantitative research, specify which CSV values will be cross-checked against authoritative sources.
