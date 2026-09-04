# mbse-system-context-checklist-reviewer — example

**What this skill produces:** A confirmation-review checklist xlsx over an MBSE system-context workbook — **6 checks** (`CR01` … `CR06`) on **3 tabs** (Title, Summary with dashboard tiles, Detailed Findings), counted from `scripts/check_definitions.py` on 2026-09-04. The checks confirm actor inventory, external interface definition, use-case population, operational scenarios, and stakeholder-needs capture.

**Typical input shape:** A system-context workbook xlsx — normally the output of `mbse-system-context-builder`, best-effort on others. `scripts/context_probe.py` reads `Title`, `Actor Inventory`, `External Interfaces`, `Use Case Catalog`, `Operational Scenarios` and `Stakeholder Needs`. All six names are emitted by the paired builder (confirmed by execution 2026-09-03 — the `Stakeholder Needs` tab is created through a dynamic `create_sheet(sheet_name)`, which is why a static scan could not resolve it).

**Expected output:** `<name>_checklist.xlsx` (default suffix when no output path is given) with every check rated FC / LC / PC / NO / NA and a dashboard verdict. The source workbook is never modified.

**Sample I/O:**

```bash
python scripts/generate_context.py examples/mbse-system-context-builder/sample_input.json acc_context.xlsx
python scripts/generate_checklist.py acc_context.xlsx acc_context_checklist.xlsx
```

Observed spreads (2026-09-03): ACC sample input → 5 FC / 1 PC; `{}` input → 4 NO / 1 PC / 1 FC. `CR05` (stakeholder needs) cannot pass on the builder's own output today because the builder drops the `stakeholder_needs` key and writes a placeholder tab — that is a builder defect (polish log, HIGH), not a reviewer one.

**Paired builder:** `mbse-system-context-builder`. This reviewer crashed on every input until 2026-09-03 (#58, `dashboard.py:138`); the same one-line fix was applied to the two sibling mbse reviewers.

> **Doc drift found 2026-09-04 (DOCS run, not fixed):** `SKILL.md` advertises "25+ checks"; `check_definitions.py` defines **6**. Also latent: `dashboard.py` counts major issues only when `obligation == "Shall"` while every check here uses `Must`/`Should`, so `REJECTED` can never fire and a `{}` workbook with 4 NO reports `CONDITIONAL APPROVAL`. Both are archive changes — POLISH-mode work, not a Friday docs change.
