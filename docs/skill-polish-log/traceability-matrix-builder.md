# Polish log — traceability-matrix-builder (+ traceability-matrix-checklist-reviewer)

## 2026-09-01 — first pass (POLISH, W36 Tuesday slot, issue #52)

**Severity: HIGH.** Both halves of the pair are non-functional in the way that matters, and the
reviewer half fails in the specific direction that produces a false pass on an audit artifact.
This is the [#54](https://github.com/jherrodthomas/automotive-skills-suite/issues/54)
"launders-into-a-passing-audit" shape, worse than #54, and it is the first `v&v` skill ever
opened.

Nothing was repaired. See **Why nothing was fixed** at the end — that is the load-bearing part
of this entry.

### What's good

- Both archives are intact (`testzip()` clean), all `.py` files `py_compile` cleanly, and there
  is no `__pycache__` contamination.
- Frontmatter is well-formed on both: `name` and `description` present, correctly named, 403 and
  338 chars — comfortably inside the 1,024 limit.
- `recalc.py` is byte-identical to the repo-wide single hash (`530f1af3…`, 5,782 bytes) in both
  archives. Confirmed repo-wide today: **1 distinct hash across all 152 archives.**
- All 5 emitted sheet names are Excel-legal (≤31 chars, no `[]:*?/\`).
- `sample_input.json` did not exist; one was written from the SKILL.md's stated schema and is
  now committed under `examples/traceability-matrix-builder/`.
- The **25 check definitions are genuinely good content.** `TM001`–`TM025` are the right
  measures for an RTM, correctly categorised, and correctly cite ISO 26262 / ISO 21434 /
  IEEE 1012. The specification is sound; only the implementation is absent.
- `dashboard.py` is the shared 16 KB charting module and is itself fine.

### What to fix

**1. The builder discards its input entirely. (HIGH)**

`generate_trace.py` opens and parses the JSON, assigns `item = data.get("item", {})` at line 19,
and never reads `item` or `data` again. Verified by execution: the sample input above — 2 needs,
4 requirements across 3 levels, 1 design element, 2 test cases — produces a workbook in which
none of those IDs appear. `{}` produces the same bytes. `from datetime import date` is imported
and unused, the same dead-import tell seen in #54.

**2. The builder emits 5 of the 11 tabs its own SKILL.md advertises. (HIGH)**

Emitted: `Title`, `Forward Traceability`, `Backward Traceability`, `Coverage Analysis`,
`References` — one heading cell each, five cells in the whole workbook. Missing: `Document
Control`, `Trace Source Catalog`, `Trace Quality Metrics`, `Gap Identification`, `Trace
Convention Rules`, `Validation Rules`. The six missing tabs are precisely the six that would
have held content.

**3. The reviewer certifies an empty document as compliant. (HIGH — the real finding)**

Run against the builder's own unedited output: **2 FC / 22 LC / 1 PC / 0 NO**, Findings tab
empty. It returns `TM005 "No orphan requirements identified"` and `TM006 "No orphan test cases
identified"` at **Fully Compliant** for a workbook containing no requirements and no test cases.

- 22 of 25 checks are a hard-coded `rating = "LC"` fall-through that never reads `probed`.
- `TM005`/`TM006` are `FC if len(list) == 0` — absence of evidence scored as compliance.
  `orphan_tests` is **never assigned anywhere in `trace_probe.py`**; it is a dead dataclass
  field, so `TM006` is FC unconditionally, for every input, forever.
- `TM007` "Overall coverage >= 95%" is `FC if len(elements) > 0` — a coverage threshold decided
  without reading a coverage number. It only scored PC here because the catalog tab is absent;
  give it one row and it certifies 95% coverage.
- `CheckResult` in `check_definitions.py` is declared and unused.

**4. Builder-to-reviewer sheet-name break. (MED)**

`trace_probe.py` reads `Trace Source Catalog` and `Coverage Analysis`. The builder never emits
`Trace Source Catalog`, so `probed.elements` is structurally always empty. Both files are dated
2026-05-02 — they shipped mismatched on day one.

**5. `build_dashboard` imported, never called, and shape-incompatible. (MED)**

`generate_checklist.py` line 14 imports it and hand-rolls a 4-row text Dashboard instead;
verified **0 charts, 0 images** in the output, against a SKILL.md promising "coverage pie
charts". Wiring it up is not a one-line call: `dashboard.py` consumes
`{tab_code: [(check_def, check_result), …]}` with attribute access, and this reviewer builds
`{check_id: {"rating": …}}`. It needs an adapter.

### Repo-wide finding — this falsifies the #46 descope premise

`docs/chain-contract-audit.md` reports **0 BREAK** and excludes builder-to-reviewer pairs, on the
stated ground that _"a reviewer ships with the builder it reviews, so the two cannot drift apart
the way #43 did."_ **Today's pair is a counter-example**, so a scan was run across all 76 pairs
for reviewer-hard-coded sheet names absent from the paired builder's `create_sheet` literals:

| Pair | Verdict |
|---|---|
| `traceability-matrix` | **BREAK — proven by execution.** `Trace Source Catalog` never emitted |
| `test-case-catalog` | **BREAK — clear.** Reviewer probes `Test Case Inventory`; builder emits `Title`, `Test Cases`, `References` |
| `flexray-config` | **BREAK — clear.** Reviewer probes `Title`; builder emits `Title_Page` |
| `mbse-system-context` | **Uncertain.** Probes `Stakeholder Needs`; builder has one dynamic `create_sheet(sheet_name)` that may or may not produce it — **do not claim without running it. This is Thursday's #58 target; check it there.** |
| `control-plan` | **False positive.** Builder names every tab via `create_sheet(tab_name)`; static scan cannot see through it |

So: **at least 3 confirmed builder-to-reviewer breaks, 1 unresolved, 1 false positive** — against
an audit that reports zero because it does not look. The exclusion is not a safe simplification.
43 of 76 reviewers hard-code sheet names, so the uncovered surface is more than half the repo.
This is direct evidence for the open question on #46 ("close as-is or re-scope"): **re-scope.**

### Why nothing was fixed

Every defect above is an authoring job, not a polish edit. Items 1 and 2 mean writing six tabs
and a full input-to-workbook mapping; item 3 means implementing 22 checks; item 5 means writing a
results adapter. The task file says to stop short of changes that need hard thought, and the
2026-08-27 run set the governing precedent when it declined to fix `BDD-12` for exactly the
reason that applies here: **repairing a check makes the builder fail its own reviewer, and that
deserves to be a decision rather than a side effect of a Tuesday.** Correcting `TM005`/`TM006`
alone would flip this builder's own output from 0 NO to 2 NO. That is the right end state, but it
should land in the same change as the builder repair, not before it.

No `.skill` archive was modified today.

### Suggested edits, in dependency order

1. Wire `sample_input.json` through `generate_trace.py` and emit all 11 tabs — the `Trace Source
   Catalog` first, since the reviewer already reads it and three other defects resolve behind it.
2. Populate `orphan_reqs` / `orphan_tests` in `trace_probe.py` from a real `Coverage Analysis`
   tab, then correct `TM005`/`TM006` so empty means **NO**, not FC, and `TM007` so it reads a
   coverage figure. Land with step 1.
3. Implement the remaining 22 checks against the probed content, or downgrade them to explicit
   `auto-suggest` drafts so the LC is not presented as a machine verdict. Either is honest; the
   current state is not.
4. Write a `results` adapter and call `build_dashboard`, or drop the pie-chart claim from the
   SKILL.md. Lowest priority; cosmetic next to the above.

**Severity: HIGH.** Do not put this pair's output in front of an assessor.
