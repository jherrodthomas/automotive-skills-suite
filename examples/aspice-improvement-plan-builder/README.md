# aspice-improvement-plan-builder — Example

**What this skill produces:** An ASPICE process improvement plan xlsx (11 tabs) that turns gap-analysis results into an actionable improvement program — initiatives with owners and target capability levels, a gaps echo tab, plus roadmap, resources, risks, KPIs, communication, and pilot tabs.

**Typical input shape:** An upstream gap-analysis workbook (e.g. an `aspice-gap-analysis-builder` output) plus a JSON per `scripts/generate_aspice_improvement_plan.py` giving, per process, the initiatives, priorities, owners, and target CLs (see `examples/sample_improvement_plan_input.json`).

**Expected output:** `<project>-aspice-improvement-plan.xlsx` — 11-tab workbook with initiative rows populated from the JSON and the gaps tab echoing the in-scope processes.

**Sample I/O:** Input the bundled sample JSON → Output an improvement plan whose initiative tabs list each improvement with priority and owner, ready to drive a CL2/CL3 uplift program.

**Run:** Trigger by phrasing, e.g. "Build the improvement plan from the brake ECU gap analysis" — then `python scripts/generate_aspice_improvement_plan.py <gap_analysis.xlsx> <input.json> <output.xlsx>`.

**Known limitations (logged 2026-07-15, see `docs/skill-polish-log/aspice-improvement-plan-builder.md`):** the `gap_analysis.xlsx` argument is currently not read (the gaps tab is filled from the JSON `processes` key), and tabs 04–09 (roadmap/resources/risks/KPIs/comms/pilot) are header-only scaffolds pending data plumbing.

**Paired reviewer:** `aspice-improvement-plan-checklist-reviewer` — audits initiative completeness, target-CL justification, and plan coverage.
