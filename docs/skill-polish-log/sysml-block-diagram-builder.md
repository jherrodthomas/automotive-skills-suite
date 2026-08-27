# sysml-block-diagram-builder — polish log

## 2026-08-27 (POLISH pass 1) — severity: **high**, changes applied

First pass on this skill and the first pass on the **sysml** domain. Slot assigned by
`WEEK-2026-W35.md` (Thursday target, issue #56 — sysml, one of the last two never-polished
domains). Target taken straight from the plan with no re-derivation: POLISH rule (1) would
otherwise re-target the ten DoD-met issues sitting in the open list, which is the same
hand-filter every Monday plan has had to apply since W33.

The pass was scoped to one pair and ended up touching four, because the headline defect turned
out to be a domain-wide copy-paste. That is recorded in full below.

### DoD from #56, item by item

- **Frontmatter validated** — builder: `name: sysml-block-diagram-builder`, description 378
  chars; reviewer: `name: sysml-block-diagram-checklist-reviewer`, description 276 chars. Both
  well under the 1024 limit, both required keys present, no extras. Clean, no change needed.
- **Generator smoke-tested from its own sample input** — **could not be done as written, and
  the reason is finding 1 below.** This builder ships no sample input and has no way to accept
  one. It was smoke-tested from its only real surface (`--title` / `--author`) and runs clean.
- **Every sheet name confirmed Excel-legal** — 11 tabs, longest `5_Part_Reference_Properties`
  at 27 characters, none containing `[ ] : * ? / \`, no leading or trailing quote. Reviewer
  output likewise (longest `1_Confirmation_Review`, 21). Pass, no change needed.
- **`recalc.py` size confirmed against the repo norm** — 5,782 bytes in both archives, matching
  the single repo-wide hash established by #55. Pass, no change needed.
- **Reviewer pairing confirmed by actually opening a builder output** — done, and this is where
  the pass earned its keep. See below.
- **Polish log** — this file.
- **Example README** — `examples/sysml-block-diagram-builder/README.md`, written from the
  verified run.

### What was fixed — the reviewer could not run at all

`block_probe.py` line 71 read:

```python
sheet_names=[ws.title for ws in wb.sheetnames if ws],
```

`wb.sheetnames` is a list of **strings**, not worksheets. So `ws.title` is the unbound
`str.title` **method object**, never called, and `sheet_names` was populated with 11 bound
methods instead of 11 names. Five lines later the probe calls `sheet_name.lower()` on those,
and dies:

```
AttributeError: 'builtin_function_or_method' object has no attribute 'lower'
```

This is not an edge case. It fires on **every input**, including the builder's own output, on
the first sheet, before any check runs. The paired reviewer had a 0% success rate against
anything. Fixed to the obvious one-liner:

```python
sheet_names=list(wb.sheetnames),
```

Measured before/after, executed from the committed archives rather than the working tree:

| | before | after |
|---|---|---|
| `generate_checklist.py` on builder output | `AttributeError`, no file written | exit 0, checklist written |
| checks executed | 0 of 15 | 15 of 15 |
| blocks / ports / connectors probed | — | 5 / 4 / 2 |
| ratings | — | 10 FC, 1 PC, 4 auto-suggest, 0 NO |

The single PC is real and correct: BDD-07 flags `Subsystem_B` and `ExternalActor` as
unreferenced by any connector, which they are.

### The same line was in three more archives

A repo-wide scan for the idiom found it in **all four** sysml reviewers, byte-identical:

- `sysml-block-diagram-checklist-reviewer` — live crash, the one above
- `sysml-activity-diagram-checklist-reviewer` — latent
- `sysml-requirement-diagram-checklist-reviewer` — latent
- `sysml-state-machine-checklist-reviewer` — latent

In the other three the field is written and never read, so nothing crashes today: it is a
landmine, not a failure. All four were fixed anyway, because the substitution is identical,
proven, and zero-behaviour-change in three of them, and because #55 set the precedent that a
mechanical one-line batch fix is a polish-pass change. Repo-wide offenders: **4 -> 0 of 152.**

Scope note: extending past #56's single pair was a judgement call. Leaving three copies of a
line already known to crash — under a read that does not exist *yet* — is worse than a
four-file diff, and the alternative of filing it runs against the standing request to shrink
the open count rather than grow it.

### Found, NOT fixed (deliberate descope)

**1. The builder cannot accept a system description. HIGH.**
`generate_sysml_block.py` takes `output`, `--title` and `--author`. Nothing else. Every block,
property, part, port, connector, item flow and value type in the output is **hard-coded
placeholder text** — `System`, `Subsystem_A`, `Subsystem_B`, `Component_A1`, `ExternalActor`.
`import json` sits at line 14 and is never used. An analyst who describes a real brake-by-wire
architecture gets a workbook about `Subsystem_A`, with their own title on the cover.

This is a genuine outlier against the repo norm, not a house style. Scan of all 76 builders:

| | count |
|---|---|
| builders that actually `json.load` an input file | **71** / 76 |
| import `json`, never load it (dead import) | 2 — `sysml-block-diagram-builder`, `communication-matrix-builder` |
| no json at all | 3 — `sysml-activity-diagram`, `sysml-requirement-diagram`, `sysml-state-machine` |

So **all four sysml builders are placeholder-only**, and they are four of the five outliers in
the entire repo. Not fixed here: wiring an input schema through eight tab-builders means
authoring a schema and a fixture, which is a build, not a polish. It is the most valuable piece
of work available in this domain and should be scoped as its own target.

**2. Three of the four sysml reviewers are stubs that advertise 25 checks. HIGH.**
Their `check_definitions.py` is 343 bytes and contains **zero** `CheckDef` entries; their probe
is 603 bytes and extracts nothing but sheet names and a format flag; `element_count` is
hard-coded to 0. Measured output, after today's fix, from the committed archives:

| reviewer | SKILL.md claims | actual checks | checklist tabs emitted |
|---|---|---|---|
| sysml-block-diagram | 28 checks / 7 tabs | **15** | 3 |
| sysml-activity-diagram | 25 checks / 7 tabs | **0** | **1** (title only) |
| sysml-requirement-diagram | 25 checks / 7 tabs | **0** | **1** (title only) |
| sysml-state-machine | 25 checks / 7 tabs | **0** | **1** (title only) |

Three reviewers exit 0 and hand back a single title tab. They do not fail — they succeed
emptily, which is the failure mode this repo has already been bitten by twice (#54's C005, and
the silent fmeda-to-TSC break filed as #53). Not fixed: writing 75 checks is authoring.

**3. Check-count drift on the one real reviewer. LOW.**
`sysml-block-diagram-checklist-reviewer` SKILL.md claims "28 canonical checks across 6
confirmation review tabs". Reality is 15 checks, all on one tab (`CR`). Same drift the W34 docs
pass found on two other reviewer stubs. Number left alone until the checks exist, so the gap
stays documented as a gap rather than being papered over as a shortfall.

**4. BDD-12 only validates connector *source* ports. MED — launders into a passing audit.**

```python
for conn in p.connectors:
    if "." in conn.source_port:   # target_port is never examined
```

The builder's own placeholder data references two target ports that do not exist —
`Subsystem_A.data_out` and `Component_A1.service_req`, neither present in the Port Catalog —
and BDD-12 returns **FC, confidence High**. Exactly the #54 shape: a Shall-check reporting
compliance on input it never looked at. The fix is a symmetric six-line block, but it makes the
builder's own output fail its own reviewer, and that should be a decision taken deliberately
rather than a side effect of a Thursday. Recorded here, not applied.

**5. `build_dashboard` is imported and never called. LOW.**
`generate_checklist.py` line 14 imports it; nothing calls it. Both SKILL.md files advertise a
"Dashboard with KPIs" and "Pie/bar charts"; no chart object is constructed anywhere in the
archive. Only `CR`-tab results are written to the workbook at all — results computed for any
other tab are discarded.

**6. Shadowed loop variable in `bdd_12`. Cosmetic.**
`[p.name for p in p.ports.get(block, [])]` shadows the `p: ProbedBlockDiagram` parameter. Safe
today only because comprehensions carry their own scope in Python 3. Left alone: renaming it
inside a check that finding 4 is going to rewrite anyway is churn.

### Verification

Everything above was measured from the **committed** `.skill` archives, extracted into scratch
tempdirs outside the archive tree (per the `__pycache__` contamination lesson from #54):

- all 4 sysml builder-to-reviewer pairs run end to end, exit 0
- `py_compile` clean on all 11 Python members of the two block-diagram archives
- `testzip()` clean on all **152** archives; `__pycache__` contamination **0 of 152**
- `scripts/chain_contract_audit.py`: 46 assertions, 16 chains, **0 BREAK** (unchanged)
- `scripts/regen_status.py`: 76 builders / 76 reviewers / 100% paired
