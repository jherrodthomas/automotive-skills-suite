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

---

## 2026-09-08 — repair pass (POLISH, W37 Tuesday slot, issue #59)

**Severity after this pass: MEDIUM** (was HIGH). The pair is functional and no longer certifies an
empty document. What remains is honesty-of-labelling on 20 checks, now explicitly labelled rather
than silently wrong.

Executed in the dependency order the 09-01 entry wrote down, and for the reason it gave: the
`TM005`/`TM006` correction had to land in the same change as the builder repair, because fixing the
checks first would have made the builder fail its own reviewer for the wrong reason.

### 1. Builder — input is now wired through to all 11 tabs (was: 5 stub tabs, input ignored)

`generate_trace.py` grew from a 55-line stub that read `data` and then emitted five empty headings
into a generator that flattens every input list into one catalog and derives the rest. All 11 tabs
promised by SKILL.md are emitted and populated: `Title`, `Document Control`, `Trace Source
Catalog`, `Forward Traceability`, `Backward Traceability`, `Coverage Analysis`, `Trace Quality
Metrics`, `Gap Identification`, `Trace Convention Rules`, `Validation Rules`, `References`.

Coverage model, written onto the `Validation Rules` tab so an assessor can read the definition the
numbers were computed under: a requirement is covered when at least one lower-level requirement,
design element or test case references its ID; a test case is an orphan when it verifies no ID
present in the catalog.

Verified against `examples/traceability-matrix-builder/sample_input.json`:

```
catalog=9  orphan_reqs=1  orphan_tests=1  overall=75.0%
```

The DoD's cell scan passes — `"PedalPlausibility module"`, a string that exists only in the sample
input, is found at `Trace Source Catalog!C8`. The sample turns out to be well-chosen: it carries
exactly one orphan requirement (`SYS-REQ-002`, nothing downstream of it) and one orphan test
(`TC-900`, `verifies: []`), so it exercises both failure paths rather than producing a clean pass.

### 2. Reviewer probe — sheet-name break closed, orphans and metrics now actually read

`Trace Source Catalog` is now emitted, so `probed.elements` is no longer structurally empty. This
closes the pair break proven by execution on 09-01.

`trace_probe.py` previously read orphans from a fixed `min_row=6, max_row=7` window and never
assigned `orphan_tests` anywhere. It now locates both the metric block and the orphan register **by
label in column A**, not by row offset, so inserting a row in the builder cannot silently empty the
probe again — which is precisely how the old window failed. `orphan_tests` and `orphan_needs` are
populated, `metrics` and `sheetnames` are new fields.

### 3. Checks — TM005/TM006/TM007 corrected, TM008/TM009 implemented, the rest labelled

The three named in the DoD, plus two that came free once the metric block was readable:

| Check | Was | Now |
|---|---|---|
| TM005 | `FC if len(orphan_reqs)==0` — absence of evidence read as compliance | `NO` with the offending IDs; `NA` with an explicit "not assessable" finding when the catalog is empty |
| TM006 | `FC` unconditionally, forever (`orphan_tests` never assigned) | same treatment, against a field that is now populated |
| TM007 | `FC if len(elements) > 0` — a coverage threshold decided without reading a coverage number | reads `overall_coverage` and compares to 95% |
| TM008 / TM009 | hard-coded `LC` | read `requirement_coverage` / `test_coverage` against 100% / 95% |
| TM003/4, TM014, TM016, TM017, TM018, TM024 | hard-coded `LC` | structural presence check on the required tab; `NO` when absent |
| remaining 12 | hard-coded `LC`, presented as a machine verdict | still `LC`, now carrying `AUTO-SUGGEST DRAFT - not machine-verified` in the finding, and `DRAFT` in a new evidence field |

Each result now carries an `evidence` string tagged `CONTENT` / `STRUCTURE` / `DRAFT` so the reader
can tell which of the three produced the rating. `generate_checklist.py`'s Findings tab was
widened to `ID | Confirmation Measure | Finding | Evidence` — it previously wrote a bare sentence
with no check ID attached, which made a finding untraceable back to its measure.

### 4. Measured result

| Input | Before (09-01) | After |
|---|---|---|
| Builder's own output | 2 FC / 22 LC / 1 PC / 0 NO, Findings tab **empty** | **0 FC / 20 LC / 5 NO**, five findings with IDs and evidence |
| Deliberately empty workbook | *(certified as above)* | **0 FC / 13 LC / 7 NO / 5 NA** — refuses to rate what it cannot see |

The builder now fails its own reviewer, on five real defects present in the sample input. That is
the correct end state and it was the stated reason for landing both halves together.

Round-trip verified: both `.skill` archives were repacked, re-extracted to a clean directory, and
run end to end from the extracted copies.

### Not done — carried

1. **The 12 remaining `DRAFT` checks.** Labelled honestly but still not implemented. TM010
   (ID convention conformance) and TM019 (dangling cross-references) are the two now cheap to do —
   the builder already computes `dangling` and writes the convention table — and should be the
   next slice.
2. **`build_dashboard` is still imported and not called** (`generate_checklist.py` line 14), and
   the hand-rolled 4-row text Dashboard is unchanged. Still needs the results adapter described on
   09-01; the SKILL.md pie-chart claim is still unmet. Deliberately left out to keep this change
   to the DoD.
3. **`docs/chain-contract-audit.md` still reports 0 BREAK and still excludes builder-to-reviewer
   pairs.** This pair was the counter-example that falsified the exclusion, and it is now repaired
   — but the *audit* has not been re-scoped, and the two other confirmed pair breaks
   (`test-case-catalog`, `flexray-config`) are Thursday's #61. The #46 re-scope decision is still
   waiting on a human.
