# Polish log — aspice-improvement-plan-builder

## 2026-07-15 (autonomous POLISH pass)

**What's good**
- Frontmatter complete (name + description, 513 chars, well under 1024).
- Clean SKILL.md: clear 6-step workflow, 11-tab output table matches generated workbook exactly.
- Scripts compile; sample JSON valid; consistent styling helpers (navy/light-blue theme shared with sibling ASPICE builders).
- Smoke test: 11 tabs generated, initiative rows populated, no exceptions.

**What to fix**
1. **Ignored `gap_analysis.xlsx` argument (HIGH, chain-break).** The CLI requires
   `<gap_analysis.xlsx>` but `generate_improvement_plan()` never opens it —
   `load_workbook` is imported and unused. Generation succeeds with a nonexistent
   path. The "02_Gaps_From_Analysis" echo tab is filled from the JSON `processes`
   key instead, so the advertised aspice-gap-analysis-builder → improvement-plan
   chain is not actually wired. Same defect class as the 2026-07-01 finding on
   aspice-gap-analysis-builder (ignored-arg).
2. **Tabs 04–09 are header-only scaffolds (MED).** `create_roadmap_tab`,
   `create_resources_tab`, `create_risks_tab`, `create_kpis_tab`,
   `create_communication_tab`, `create_pilot_tab` take no data and emit only
   title + header rows. SKILL.md Steps 3–4 instruct users to define timeline,
   resources, risks, KPIs, comms, and pilot — all of which are silently dropped.
   Sample JSON lacks those keys too, masking the gap.

**Suggested edits**
- Either implement gap-xlsx ingestion (read gap rows into 02 tab, cross-check
  initiative `gaps` IDs) or drop the argument and update SKILL.md/usage — pick one.
- Extend generator to consume `risks`, `kpis`, `roadmap`, `resources`,
  `communication`, `pilot` JSON keys; extend sample JSON to exercise them.
- Add the missing keys to examples/sample_improvement_plan_input.json.

**Severity:** high (chain-break) / medium (scaffold tabs)

**Action taken this run:** log only — both fixes exceed small-fix scope
(new parsing + data plumbing). No .skill modification.
