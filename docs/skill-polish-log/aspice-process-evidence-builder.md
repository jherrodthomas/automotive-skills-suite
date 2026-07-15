# Polish log — aspice-process-evidence-builder

## 2026-07-15

**What's good**
- Frontmatter valid; description 483 chars (well under 1024 limit).
- All scripts `py_compile` clean; smoke test green: sample JSON → 10-tab workbook, JSON status output correct (SWE.1, 2 BPs, 1 GP).
- Unlike its two ASPICE siblings (gap-analysis, improvement-plan), the `<input.json>` CLI argument is **genuinely consumed** — process info, BP/GP evidence, and work products are all plumbed from JSON into tabs 00–05. Negative data point for the family-wide ignored-arg sweep: this builder is NOT affected.
- All 10 sheet names ≤31 chars (Excel limit); no reviewer-probe mismatch suspected.

**What to fix**
1. **Scaffold-only tabs 06/08, hard-coded tab 07** (same defect class logged on aspice-improvement-plan-builder 2026-07-15): `create_performance_tab`, `create_improvement_tab` render headers only and take no data; `create_quantitative_tab` hard-codes 4 metric names with no values. SKILL.md Step 4 explicitly asks users for performance records, CL3+ quantitative metrics, and improvement history — all silently dropped. Sample JSON also lacks these keys, so the gap is invisible in smoke tests.
2. Sample JSON should gain `performance_records`, `quantitative_metrics`, `improvements` keys once (1) is wired.

**Suggested edits**
- Add `performance_records`/`quantitative_metrics`/`improvements` top-level JSON keys; thread them through `generate_evidence` into tabs 06–08 following the tab-03/04/05 pattern (headers + striped body rows).
- Extend `examples/sample_evidence_input.json` with 1–2 rows per new key.

**Severity:** med — workbook generates and core evidence tabs work, but a third of the promised content (Step 4) is a silent no-op, which matters for CL3+ assessments specifically.

**Fix applied this run:** none — wiring three tabs is beyond small-fix scope per polish rules; logged for a PLAN-week fix alongside the sibling scaffold-tab findings.
