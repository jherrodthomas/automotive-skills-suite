# Polish Log — apqp-plan-builder

## 2026-06-30 (POLISH run)

**Selected by:** priority 3 (least-recently-touched builder, last touched 2026-05-01) —
no open issues, no orphan builders. First least-recently-touched builder without an
existing polish log.

**Verification performed this run:** unzipped the `.skill`, validated frontmatter,
validated the example JSON, and ran the generator end-to-end on
`examples/sample_apqp_plan_input_esc.json` (smoke test). Generator exits 0 and produces
a 15-tab workbook.

### What's good
- Frontmatter is clean: `name` (17 chars) and `description` (391 chars, well under the
  1024 limit) both present and well-formed. Description has strong trigger coverage
  (APQP, product quality planning, automotive supplier planning).
- SKILL.md is well-structured: clear "When to use", a 5-step workflow, an output-structure
  table, AIAG standard-deliverable counts per phase, and a file map.
- Skill is functional: example JSON is valid and the generator runs cleanly to a 15-tab
  xlsx with no errors. JSON-as-source-of-truth guidance is sound.
- Forgiving schema (defaults for omitted fields; only `program.name` + `program.customer`
  mandatory) is documented consistently.

### What to fix
1. **[MED] Sheet names exceed Excel's 31-char limit.** The generator emits phase tabs such
   as `02_Phase2_Product Design and Development` (40 chars). openpyxl warns: *"Title is
   more than 31 characters. Some applications may not be able to read the file."* Excel
   truncates or rejects these on open. Fix: shorten tab names to ≤31 chars (e.g.
   `04_Phase2_Product Design` or `04_P2_Product Design & Dev`).
2. **[LOW] Tab-number mismatch between SKILL.md and actual output.** The "Output structure
   (15 tabs)" table documents the phase tabs as `04`–`08` (04 Phase 1 … 08 Phase 5), but the
   generator actually prefixes them `01_Phase1` … `05_Phase5`, which also visually collides
   with `01_Document_Control`. Align the generator's numbering with the documented `04`–`08`
   scheme (or update the table to match the code).

### Suggested edits (deferred — require re-packaging the .skill archive)
- In `scripts/generate_apqp_plan.py`, rename the five phase worksheets to ≤31-char,
  `04`–`08`-prefixed titles. This resolves both findings in one edit and keeps SKILL.md's
  output table accurate.

### Severity
- Overall: **low-to-medium**. The skill works today; the 31-char issue is a real Excel
  compatibility risk worth a targeted fix, but the change touches code inside the packaged
  `.skill` zip and is descoped from this POLISH run per "small and shipped beats big and
  broken." Captured as a follow-up below.

### Follow-ups
- [ ] Apply the phase-tab rename (≤31 chars, `04`–`08` prefixes) in
  `scripts/generate_apqp_plan.py`, re-zip the `.skill`, and re-run the smoke test.
- [ ] Consider filing this as a `skill-bug` issue so it surfaces in the next POLISH priority pass.
