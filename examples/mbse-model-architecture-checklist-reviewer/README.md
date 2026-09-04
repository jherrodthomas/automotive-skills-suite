# mbse-model-architecture-checklist-reviewer — example

**What this skill produces:** A confirmation-review checklist xlsx over an MBSE model-architecture workbook — **8 checks** (`CR01` … `CR08`) on **3 tabs** (Title, Summary with dashboard tiles, Detailed Findings), counted from `scripts/check_definitions.py` on 2026-09-04. The checks confirm that each of the four ARCADIA layers is populated and that inter-layer traceability holds.

**Typical input shape:** A model-architecture workbook xlsx — normally the output of `mbse-model-architecture-builder`, best-effort on others. `scripts/mbse_probe.py` reads `Title`, `Operational Analysis`, `System Analysis`, `Logical Analysis` and `Physical Analysis`; all five are emitted by the paired builder under exactly those names.

**Expected output:** `<name>_checklist.xlsx` (default suffix when no output path is given) with every check rated FC / LC / PC / NO / NA and a dashboard verdict. The source workbook is never modified.

**Sample I/O:**

```bash
python scripts/generate_checklist.py model_architecture.xlsx model_architecture_checklist.xlsx
python scripts/recalc.py model_architecture_checklist.xlsx      # optional
```

No `examples/mbse-model-architecture-builder/sample_input.json` exists yet — this reviewer was verified on 2026-09-03 only against its builder's `{}` output (runs to completion, ratings populate). A domain-real sample is a W37 item.

**Paired builder:** `mbse-model-architecture-builder`. This reviewer crashed on every input until 2026-09-03 (#58, `dashboard.py:138`, flat list passed where `{tab: [(check, result)]}` was expected); fixed alongside the other two mbse reviewers.

> **Doc drift found 2026-09-04 (DOCS run, not fixed):** `SKILL.md` advertises "30+ checks across four ARCADIA layers"; `check_definitions.py` defines **8**. Same `Shall`-vs-`Must` obligation mismatch in `dashboard.py` as the sibling reviewers (by grep, not yet verified by execution). Archive changes — POLISH-mode work.
