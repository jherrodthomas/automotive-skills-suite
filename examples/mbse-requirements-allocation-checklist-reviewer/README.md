# mbse-requirements-allocation-checklist-reviewer — example

**What this skill produces:** A confirmation-review checklist xlsx over an MBSE requirements-allocation workbook — **6 checks** (`CR01` … `CR06`) on **3 tabs** (Title, Summary with dashboard tiles, Detailed Findings), counted from `scripts/check_definitions.py` on 2026-09-04. The checks confirm requirement catalog completeness, allocation-matrix population, and that every requirement is allocated to at least one architectural element.

**Typical input shape:** A requirements-allocation workbook xlsx — normally the output of `mbse-requirements-allocation-builder`, best-effort on others. `scripts/reqs_alloc_probe.py` reads `Title`, `Requirements Catalog` and `Allocation Matrix`; all three are emitted by the paired builder under exactly those names.

**Expected output:** `<name>_checklist.xlsx` (default suffix when no output path is given) with every check rated FC / LC / PC / NO / NA and a dashboard verdict. The source workbook is never modified.

**Sample I/O:**

```bash
python scripts/generate_checklist.py reqs_allocation.xlsx reqs_allocation_checklist.xlsx
python scripts/recalc.py reqs_allocation_checklist.xlsx      # optional
```

No `examples/mbse-requirements-allocation-builder/sample_input.json` exists yet — this reviewer was verified on 2026-09-03 only against its builder's `{}` output (runs to completion, ratings populate). A domain-real sample is a W37 item.

**Paired builder:** `mbse-requirements-allocation-builder`. Upstream chain: `mbse-model-architecture-builder` supplies the logical/physical elements the allocation targets. This reviewer crashed on every input until 2026-09-03 (#58, `dashboard.py:138`); fixed alongside the other two mbse reviewers.

> **Doc drift found 2026-09-04 (DOCS run, not fixed):** `SKILL.md` advertises "28+ checks"; `check_definitions.py` defines **6**. Same `Shall`-vs-`Must` obligation mismatch in `dashboard.py` as the sibling reviewers (by grep). Archive changes — POLISH-mode work.
