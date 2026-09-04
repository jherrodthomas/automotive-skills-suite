# sysml-block-diagram-checklist-reviewer — example

**What this skill produces:** A confirmation-review checklist xlsx over a SysML block-diagram workbook — **15 checks** (`BDD-01` … `BDD-15`) on **3 tabs**, counted from `scripts/check_definitions.py` and `generate_checklist.py` on 2026-09-04: Title, `1_Confirmation_Review` (one row per check with rating, confidence, finding, recommended action) and `2_Summary` (dashboard tiles and rating breakdown). Checks cover block well-formedness, port typing, connector completeness (both ends resolve — `BDD-12`, repaired 2026-09-02), and item-flow semantics per OMG SysML 1.6 / 2.0.

**Typical input shape:** A block-diagram workbook xlsx — normally the output of `sysml-block-diagram-builder`, best-effort on other BDD/IBD-shaped workbooks. `scripts/block_probe.py` reads the numbered tabs the builder emits (`1_Document_Control`, `2_Model_Header`, `3_Block_Catalog`, `4_Property_Catalog`, `5_Part_Reference_Properties`, `6_Port_Catalog`, `7_Connector_Catalog`, `8_Item_Flow_Catalog`, `9_Value_Type_Definitions`, `10_References`) and flags `is_builder_format` when the sheet names carry the numeric prefix.

**Expected output:** `<name>_checklist.xlsx` with every check rated FC / LC / PC / NO / NA plus a finding and recommended action. The source workbook is never modified.

**Sample I/O:**

```bash
# build a workbook from the brake-by-wire sample, then review it
python scripts/generate_sysml_block.py --input examples/sysml-block-diagram-builder/sample_input.json bbw_bdd.xlsx
python scripts/generate_checklist.py bbw_bdd.xlsx bbw_bdd_checklist.xlsx
python scripts/recalc.py bbw_bdd_checklist.xlsx      # optional; detects formula errors
```

A builder run with **no** `--input` now emits an empty template (behaviour change under #57, 2026-09-02); reviewing that file correctly scores `BDD-02 NO` — that is the reviewer working, not a bug. A deliberately dangling connector target in the sample is caught by `BDD-12` at High confidence.

**Paired builder:** `sysml-block-diagram-builder`. Upstream: none (a BDD is a root deliverable); `sysml-activity-diagram`, `sysml-requirement-diagram` and `sysml-state-machine` are siblings queued for the same JSON-input treatment in W37.

> **Doc drift found 2026-09-04 (DOCS run, not fixed):** `SKILL.md` says "28 canonical checks across 6 confirmation review tabs" and "7-tab checklist xlsx". `check_definitions.py` defines **15** checks and the generator creates **3** tabs. Left unfixed deliberately: correcting it means repacking the archive, which is POLISH-mode work (standing item 9 — whether this task authors the missing 13 checks or downgrades the doc).
