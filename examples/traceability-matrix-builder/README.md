# traceability-matrix-builder — Example

**What this skill produces:** Nominally an 11-tab bidirectional requirements traceability matrix
(needs → system → subsystem → component → design → test → results) per ISO 26262, ISO 21434 and
IEEE 1012. **In reality, verified by execution on 2026-09-01, it produces a 5-tab workbook
containing 5 cells of literal text and none of your data.** Read the rest of this file before
using it.

**Typical input shape:** A JSON file. See `sample_input.json` in this directory — stakeholder
needs, three requirement levels, design elements and test cases with `traces_to` / `verifies`
cross-references. The generator does `json.load` it. It then does nothing with it. The only
consumer of the parsed document is `item = data.get("item", {})` on line 19, and `item` is never
read again. **Every field you supply is discarded.** Any syntactically valid JSON — including
`{}` — produces a byte-identical workbook.

**Expected output:** 5 tabs, not the 11 the SKILL.md lists: `Title`, `Forward Traceability`,
`Backward Traceability`, `Coverage Analysis`, `References`. Each contains exactly one heading
cell and no rows. The six tabs that would carry the actual traceability content — `Document
Control`, `Trace Source Catalog`, `Trace Quality Metrics`, `Gap Identification`, `Trace
Convention Rules`, `Validation Rules` — are **never created**. The `Title` tab reads the literal
string `"Traceability Matrix"`; your project name does not reach it.

**Sample I/O:**

```bash
python scripts/generate_trace.py sample_input.json matrix.xlsx
```

prints `Traceability Matrix generated: matrix.xlsx`. Opening `matrix.xlsx` gives you 5 headings.
Neither `SN-001`, `SYS-REQ-001`, `TC-001` nor `Brake-by-Wire System` appears anywhere in the file.

**Paired reviewer:** `traceability-matrix-checklist-reviewer` — advertises 25 checks (`TM001`
through `TM025`). It runs, exits 0, and produces a 5-tab checklist. **Do not use its verdict.**
Against the empty workbook above it returns **2 FC / 22 LC / 1 PC / 0 NO** and an empty Findings
tab — that is, it certifies a document with zero requirements and zero test cases as broadly
compliant, including `TM005 "No orphan requirements identified"` and `TM006 "No orphan test cases
identified"` at **Fully Compliant**.

Why, mechanically:

- **22 of the 25 checks are hard-coded `rating = "LC"`** in `check_definitions.py` and never
  touch the probed workbook. They return the same answer for every input that has ever been or
  will ever be passed to this skill.
- **`TM005` and `TM006` invert absence of evidence into compliance.** Both are
  `FC if len(list) == 0`. `orphan_reqs` is populated only from rows 6–7 of `Coverage Analysis`,
  a tab the builder writes one cell to; `orphan_tests` is **never assigned by any code path in
  `trace_probe.py`** — it is a dead dataclass field, so `TM006` is unconditionally FC.
- **`TM007` "Overall coverage >= 95%" is `FC if len(elements) > 0`.** It returns Fully Compliant
  on a coverage requirement without reading a coverage figure, the moment the source catalog has
  a single row. It scored PC here only because the catalog tab does not exist.
- **The reviewer probes `Trace Source Catalog`, which this builder never emits.** The pair has
  been mismatched since both files were written on 2026-05-02.
- **`build_dashboard` is imported and never called.** The SKILL.md promises "coverage pie
  charts"; the Dashboard tab is 4 rows of plain text with **0 charts**. The import is also
  shape-incompatible — `dashboard.py` expects `{tab: [(check_def, check_result), ...]}` with
  attribute access, and this reviewer builds `{check_id: {...}}` — so wiring it up needs an
  adapter, not a call.

**Use it for:** the tab skeleton, and as a worked example of the input schema the generator
*ought* to consume. **Do not use it for:** anything you intend to put in front of an assessor.
Recorded in `docs/skill-polish-log/traceability-matrix-builder.md`; tracked as
[#52](https://github.com/jherrodthomas/automotive-skills-suite/issues/52).
