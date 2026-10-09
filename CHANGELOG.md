# Changelog

All notable changes to the Automotive Skills Suite. Maintained by the autonomous
daily standup. Entries are grouped by intent (feat / fix / polish / docs) and move
from `[Unreleased]` into a dated section at each weekly release.

## [Unreleased]

_W40 (2026-09-28 → 2026-10-02). Accumulating since v2026.09.W39 (2026-09-26)._

### Docs
- W40 PLAN (Mon 2026-09-28): opened #66–#70, the carried HIGH backlog (`sw-arch-builder`, `sw-sr-builder`, `sw-fmea-builder` TSC/HSI column shifts; `safety-case-builder` truncated reader; `bus-load-analysis-checklist-reviewer` truncated `CHECKS` dict) (`4da1631`)
- September monthly KPI report, `docs/monthly/2026-09.md` (`f7ece99`)
- W41 (2026-10-05 → 2026-10-09) DOCS roll (Fri 2026-10-09): **no skill, script or doc commits landed this week** — the W41 PLAN and Tue–Thu POLISH slots did not commit to the repo, so there are no `feat:` / `fix:` / `polish:` entries to group and no example README stubs owed. #66–#70 remain open and unaddressed; STATUS regenerated via `scripts/regen_status.py`.

## [v2026.09.W39] — 2026-09-26

_W39 (2026-09-21 → 2026-09-25). Accumulating since v2026.09.W38 (2026-09-19). Rolled by the Friday DOCS run (2026-09-25) and shipped by the Saturday RELEASE run (2026-09-26)._

### Fix
- **cs-goals-builder** — W39 Tue (2026-09-22, #62): `Cybersecurity_Property` was hard-coded `Confidentiality` on every CSG though the threat's STRIDE category determines it unambiguously. `_build_stride_index()` added beside the existing `_build_asset_index()` (same tab, `05_Threat_Scenarios`, already indexed by scenario ID) so the property is now derived per row; unmatched rows report `Not Determined` with a stderr warning instead of silently defaulting to `Confidentiality`. Verified on six scenarios — the shipped sample, an Information-Disclosure-stripped variant, the `09_Risk_Determination` fallback path, a TARA missing `05_Threat_Scenarios`, an unrecognised STRIDE value, and a full cell-by-cell before/after diff (`4604f67`)
- **24 checklist-reviewer archives (batch)** — W39 Wed (2026-09-23, #63): `_find_header_row`'s `>= 3` populated-cell gate hid any two-column tab from its reviewer suite-wide. Measurement showed the issue's own proposed blanket `>= 2` would have moved two already-correct headers backward (`control-plan` `02_Header_Block` row 11→3, `ppap` PSW row 17→5), so the gate ships as "prefer ≥3, fall back to 2" instead — proven zero-diff across 2,616 checklist rows built from all 24 paired outputs before/after. Neither of the two symptoms the plan named turned out to move a verdict (`cs_concept_probe` never reads the affected tab; `sw_fmea_probe` feeds only an informational count), so the issue was downgraded HIGH → MED in its closing comment. The sweep also found two dead-on-arrival skills — `safety-case-builder` and `bus-load-analysis-checklist-reviewer`, both broken since their initial commits — left unfixed this run; see Known issues (`50ba884`)
- **cs-concept-builder** — W39 Thu (2026-09-24, #65): `cs_goals_reader.py` read the CS Goals tab positionally starting at row 5 against a row-1 header. 3 of 15 goals (firmware injection, calibration tampering, CAN spoofing) were never read, every CSR's CAL echoed the goal's *text* instead of a rating, and every node inherited CAL-1. Filed MED, measured HIGH. Fixed with a header-spelling lookup table; verified from the repacked archive with all 15 goals present under the right header and 0 verdict moves across `cs-concept-checklist-reviewer`'s 29 checks. This was #64's only known-bad chain edge, watched failing before the fix landed (`6d0118f`)

### Feat
- **Column-level chain-contract audit** — W39 Thu (2026-09-24, #64): `scripts/column_contract.py` (new, 503 lines) extends `chain_contract_audit.py` from tab-existence checks to column-name and data-start-row assertions per chain edge. Landed by watching the new layer fail on #65's edge first — committed before the fix — then repairing #65 behind it. Beyond #65 it reports 13 further BREAKs, all dynamically confirmed by building the real chain rather than reading source: **`sw-arch-builder`** reads `05_TSR_Catalog` two columns off, so its `"SW" in Type` filter tests the FSR_ID and every SW-TSR is dropped (0 imported), and reads `04_Safety_Mechanism_Catalog` and the SW-partition table off by one column each; **`sw-sr-builder`** reads `06_HSI_Specification` one column right of where it should from column 2 on; **`sw-fmea-builder`**'s tab-name scan looks for `partition`/`sw_tsr`, which no TSC tab contains, and imports nothing from the TSC, silently. The entire TSC→software branch has been running on misread or empty input while the tab-level audit reported every one of those edges MATCH (`5dcce3d`)

### Docs
- W39 weekly plan (Mon 2026-09-21): opened #62–#65 from the prior Sunday's triage handoff; the `_find_header_row` candidate was measured down from "suite-wide HIGH" to "24 archives carry it, 2 have a live symptom" before being slotted, and the regression-harness item was held back deliberately to keep the week to four targets instead of eight (`ea692a7`)
- W39 DOCS roll (Fri 2026-09-25, this section): 18 example README stubs added — `apqp-plan`, `aspice-assessment`, `control-plan`, `cs-concept`, `cs-goals`, `dfmea`, `hsi`, `hw-architecture`, `hw-safety-reqs`, `pfmea`, `ppap`, `safety-case`, `safety-plan`, `sw-arch`, `sw-fmea`, `sw-hsis`, `sw-sr`, `tara` checklist-reviewers — for the 18 of #63's 24-archive batch that had no `examples/<skill>/README.md`. These are stubs sourced from each skill's own frontmatter description and confirmed builder pairing, not from execution: no real check counts or sample output, flagged as such in each file, and left for a future POLISH pass to deepen. `cs-goals-builder` and `cs-concept-builder` already had READMEs, rewritten this week during #62 and #65, and were left alone. STATUS regenerated via `scripts/regen_status.py` — 76/76 paired (2 via alias registry), 7 🟢 fresh / 69 🟡 stale / 0 orphan (this commit)

### Known issues _(found this week, deliberately not fixed)_
- **Three column-level BREAKs from #64, not yet filed as issues** — `sw-arch-builder`'s TSC reader (every SW-TSR dropped; safety-mechanism catalog and SW-partition table both misread), `sw-sr-builder`'s HSI column shift, `sw-fmea-builder`'s TSC tab-name scan matching nothing. All HIGH, all dynamically confirmed by running the real chain; exact column pairs and reproduction steps in `docs/chain-contract-audit.md`. Should be fixed by header-name lookup, the same shape as #65, not by re-counting positions
- **`safety-case-builder`** — `multi_xlsx_reader.py` truncated mid-statement (`retu`), `NameError` on any input. Rated "exemplary" by a 2026-06-17 polish pass that never ran it. A one-line verified fix is written out in `docs/skill-polish-log/reviewer-find-header-row-batch-63.md` but not applied — one skill per POLISH slot
- **`bus-load-analysis-checklist-reviewer`** — `check_definitions.py`'s `CHECKS` dict truncated at entry 18 of 28, `SyntaxError` on import. The fix is mechanical (all 28 check functions already exist; only the mapping entries are missing) but likewise not applied this run
- **`column_contract.py` cannot see the #65 edge it just fixed.** cs-concept's new header-spelling reader is the right design and a genuine blind spot for a positional-assertion audit; the gap will widen as more readers move to name-based lookup, which is why the reviewer-probe regression harness (carried, HIGH, unfiled) is still needed as the dynamic complement
- cs-concept does not model CAL-4 at all — generator, verification matrix and SKILL.md line 112 all stop at CAL-3, so 160 CAL-4 CSRs in the sample still inherit CAL-3 (MED, spotted in passing, not filed)
- `cs-goals-builder` writes goal text into `02_TARA_Echo.Threat_Description` (LOW-MED, spotted in passing, not filed)
- `cs-concept-checklist-reviewer` has no check asserting the echoed CS-Goal count equals the upstream count, or that every CSR's CAL falls in `CAL-1..4` — a genuine `reviewer-finding` candidate surfaced by #65's own fix, MED, not filed

---

## [v2026.09.W38] — 2026-09-19

_W37 + W38 (2026-09-07 → 2026-09-18). Accumulating since v2026.09.W36 (2026-09-05). Rolled by the Friday DOCS run (2026-09-18) and shipped by the Saturday RELEASE run (2026-09-19). No `v2026.09.W37` tag exists and none was backfilled — see `RELEASES.md`._

> **Five scheduled runs did not fire across these two weeks.** W37 produced only its Monday PLAN and Tuesday POLISH — the Wed/Thu POLISH slots, the Friday DOCS roll and the Saturday RELEASE are all absent from the log, so two of W37's three targets (#60 mbse wiring, #61 test-case-catalog / flexray-config) were opened and never worked. W38 is the mirror image: no `docs/weekly/WEEK-2026-W38.md` and no commits dated 2026-09-14, so the Monday PLAN never ran, but all three POLISH slots did. Those three selected by rule (c) — least-recently-touched builder — because the open-issue queue is empty (#59–#61 were closed outside the automation). **Net effect: no `v2026.09.W37` tag exists, this is the first DOCS roll since 2026-09-04, and the week's work is sound but unplanned.**

### Fix
- **traceability-matrix pair** — W37 Tue (2026-09-08, #59): the pair documented as non-functional in W36 was repaired in dependency order. `generate_trace.py` rewritten so input actually flows into all 11 advertised tabs with coverage and orphans **derived** rather than emitted as headings; `trace_probe.py` now locates metrics and orphan rows by label instead of a fixed row window, so `orphan_tests` populates. `TM005`/`TM006`/`TM007` corrected, `TM008`/`TM009` implemented, and the 12 checks that cannot be machine-verified labelled `AUTO-SUGGEST DRAFT` instead of falling through to a hard-coded `LC`. Builder output now scores `0 FC / 20 LC / 5 NO` against an empty workbook's `0 FC / 7 NO / 5 NA` — **the two spreads differ, which they did not before** (`302660e`)
- **hw-safety-reqs-builder** — W38 Tue (2026-09-15): `hw_sr_overrides` was discarded in its entirety and the run still exited 0. `derive_hw_srs_from_tsr` matched `overrides[tsr_id]` exactly, but the shipped `examples/sample_hw_sr_input_esc.json` keyed its two overrides on a stale three-segment HW-TSR ID (`TSR-001-01`) while `tsc-builder` emits four segments (`TSR-001-01-01`). The generic `<TBD: ADC_CHx>` template shipped in place of the analyst's text, element and DC% target, with a summary reporting `hw_srs_derived: 825` as though refinement had applied. Overrides now resolve, unmatched keys raise a named `WARNING` on stderr, and the JSON summary carries `overrides_supplied` / `overrides_applied` / `overrides_unmatched`. Same class as #54 — a documented input path that accepts data and drops it (`a8f430d`)
- **cs-goals-builder** — W38 Wed (2026-09-16): the builder could not read the TARA workbook its own upstream produces. `tara_reader` resolved columns against snake_case names with *positional* fallbacks; `tara-builder` emits human-readable headers (`Cybersecurity Goal`, `Risk Value`), so no alias matched for `asset` or `cs_goal_text` and both took their positional default — every CSG shipped with the goal text in the `Asset` column and a bare risk integer as its goal text. Plausible-looking and wrong, carried end to end through `cs-concept-builder` and `cs-architecture-builder`. Fixed with `_HEADER_ALIASES` / `_normalize_headers()` / `_col()` (name-first resolution, `None` rather than a guessed column where no alias matches) plus `_build_asset_index()`, which walks `05_Threat_Scenarios` and `04_Asset_Inventory` because the upstream CSG tab has no asset column at all. Also hardened against an `AttributeError` on non-string header cells (`a2af2f1`)
- **dia-checklist-reviewer** — W38 Thu (2026-09-17): three false findings on the pair's own canonical sample, all one kind — the probe assumed column names and layouts `dia-builder` does not emit, then reported a compliance failure rather than a read failure. DIAA-8 stuck at `PC` on seven fully-scheduled joint reviews (`milestone`/`participants` vs the emitted `Review Name`/`Attendees`/`Planned Date`); DIAA-14 stuck at `PC` against 6 in-scope / 5 out-of-scope bullets (`_find_header_row` needs ≥3 populated cells, `03_Item_Scope` is a two-column vertical layout); DIAA-4/5 stuck at `NO` on 10 rows because combined `A,R` cells — the builder's own documented default — were compared with `v.upper() == "A"`. Fixed via header aliases, a `_scan_vertical_scope()` fallback that only runs when the header parse finds nothing, and `_raci_tokens()`. DIAA moved `FC 6 / NO 2 / PC 2` → `FC 8 / NO 2`; **the builder itself was found healthy and untouched** (`9170271`)

### Docs
- W37 weekly plan (Mon 2026-09-07): three targets #59, #60, #61 opened. Its pre-slotting audit **killed a hypothesis three previous plans had carried** — that `dashboard.py` gating `REJECTED` on `obligation == "Shall"` while `check_definitions.py` uses `Must` made the verdict dead code suite-wide. Measured across all 76 reviewers: genuinely dead in **3** archives (all mbse), consistent in 38, no `Shall` literal at all in 11, and 24 where the obligation is carried elsewhere and which remain an open question. The batch pass collapsed into a three-line add on target 2 rather than occupying a slot (`f95f4c9`)
- W38 DOCS roll (Fri 2026-09-18): this section; three example READMEs written against the archives — `hw-safety-reqs-builder`, `cs-goals-builder`, `dia-checklist-reviewer`, plus `traceability-matrix-checklist-reviewer` carried from the skipped W37 roll. Check and tab counts re-derived from source at write time: dia reviewer 14 CR + 14 DIAA + 5 VA = 33 matches SKILL.md, cs-goals 8 tabs matches SKILL.md, hw-safety-reqs 10 tabs matches SKILL.md — **no drift found this week**, the first clean docs roll since W33. STATUS regenerated via `scripts/regen_status.py` — 76/76 paired, 8 fresh / 68 stale / 0 orphan (this commit)

### Known issues _(found this week, deliberately not fixed)_
- **`_find_header_row` requires ≥3 populated cells.** Any two-column tab in any builder is invisible to its paired reviewer, which will then report a content gap rather than a read failure. Confirmed on `dia-checklist-reviewer`; almost certainly not unique to it. Candidate for a suite-wide sweep — the DIA instance lived 4.5 months
- **`chain_contract_audit.py` is tab-level only.** It reported 0 BREAK across all 16 chains for the entire time the `tara-builder` → `cs-goals-builder` payload was corrupt, because it asserts a referenced *tab* exists and never that the columns line up. A column-level assertion is the highest-leverage tooling change currently available and would retro-catch this class suite-wide
- **`cs-goals-builder` hard-codes `Cybersecurity_Property = "Confidentiality"`** on all 15 CSGs though the STRIDE category determines it. Left alone: threading a new field through a payload schema three downstream skills read is wider than a single POLISH slot
- **`examples/sample_dia_esc.json` has a real RACI defect** — WP11 Safety Plan has two Accountable parties and no Responsible; WP12 Safety Case has `A,R` for both. Now that the reviewer reads correctly these surface as genuine NOs. Needs a human call: correct the sample, or relax DIAA-4 to allow declared joint accountability
- **`cs-concept-builder` `02_CS_Goals_Echo` header/data column shift** (MED) — spotted in passing on 2026-09-16, belongs to another skill, no issue opened because the queue tooling was not exercised this week
- **`traceability-matrix-checklist-reviewer.skill` ships three `scripts/__pycache__/*.pyc` files.** Harmless at runtime, contaminating in an archive. Recorded not fixed — repacking a `.skill` is POLISH work. Worth a suite-wide `__pycache__` scan
- **No regression harness runs builder output through the paired reviewer's probe.** Every defect fixed this week is of exactly that shape and every one survived months. Worth a PLAN target

---

## [v2026.09.W36] — 2026-09-05

_W35 + W36 (2026-08-24 → 2026-09-05). Accumulating since v2026.08.W34 (2026-08-22). Shipped by the Saturday RELEASE run (2026-09-05). No `v2026.08.W35` tag exists and none was backfilled — see `RELEASES.md`._

> **Two weeks in one section.** The scheduled run died after 2026-08-27 and missed the W35 Friday DOCS roll and the W35 Saturday RELEASE, so no `v2026.08.W35` tag exists and W35's four commits were never written up here. This DOCS run (2026-09-04) rolls both weeks. Grouped by intent, W35 first.

### Fix
- **autosar-bsw-config-builder** — W35 Tue (2026-08-25, #54): builder no longer silently drops `memory_layout` and `bus_interfaces` from its input; the paired reviewer's `C005`/`C006` are reachable again. Builder and reviewer repacked in one commit (`84ea8b4`)
- **autosar batch (5 archives)** — W35 Wed (2026-08-26, #55): canonical 5,782-byte `recalc.py` restored and the mis-generated `## Skills inventory` heading corrected in the remaining five autosar archives. Repo-wide: `recalc.py` 152/152 at one hash, bad heading 0/152 (`656bdfe`)
- **sysml reviewers (4 archives)** — W35 Thu (2026-08-27, #56): `sysml-block-diagram-checklist-reviewer` crashed on every input; one-line probe fix applied and the identical defect found and fixed in the other three sysml reviewers. Domain went from zero working reviewers to four (`6e8cce6`)
- **mbse reviewers (3 archives)** — W36 Thu (2026-09-03, #58): all three mbse reviewers crashed at `dashboard.py:138` (flat list passed where `{tab: [(check, result)]}` expected — the #56 class). One-line fix, verified by execution on each reviewer's own builder output. Domain went from zero working reviewers to three; the 2026-09-01 chain-scan's unresolved `Stakeholder Needs` row settled by execution as *not a break* (`da1082a`)

### Feat
- **sysml-block-diagram-builder** — W36 Wed (2026-09-02, #57): builder now accepts `--input <model.json>`; the five placeholder blocks are gone from the generator (no `--input` now yields an empty template with a stderr warning — **behaviour change**, see Known issues); schema documented in SKILL.md with a brake-by-wire `examples/sysml-block-diagram-builder/sample_input.json`. Paired reviewer's `BDD-12` now validates both connector ends. This is the reference implementation for the W37 sysml batch (`72eefda`)

### Polish
- **traceability-matrix-builder** + reviewer — W36 Tue (2026-09-01, #52, first `v&v` entry): documented, **nothing fixed**. Builder ignores its input and emits five heading cells; reviewer certifies that empty workbook at 24/25 compliant because 22 of 25 checks are a hard-coded `LC` fall-through. Full finding in `docs/skill-polish-log/traceability-matrix-builder.md`; repair queued for W37 in dependency order (`5660048`)
- **mbse-system-context-builder** — W36 Thu (2026-09-03, #58): first pass on the last never-entered domain. Builder populates five tabs correctly from input; seven of twelve tabs are single-cell placeholders and `stakeholder_needs` / `boundary` are silently dropped — filed HIGH in `docs/skill-polish-log/mbse-system-context-builder.md`, not fixed. `examples/mbse-system-context-builder/` (README + ACC `sample_input.json`) added (`da1082a`)
- **sysml-block-diagram-builder** — W35 Thu (2026-08-27, #56): first sysml pass; two HIGH gaps documented and deliberately not repaired (no input path — since fixed under #57 — and reviewer check-count drift) (`6e8cce6`)

### Docs
- W35 weekly plan (Mon 2026-08-24): three targets #54, #55, #56; #56 opened (`6ed7fe5`)
- W36 weekly plan (Mon 2026-08-31): deferred #52 slotted first, #57 and #58 opened (`30044b2`)
- Monthly KPI report `docs/monthly/2026-08.md` (2026-09-01): 29 commits, 17 archives touched, 3 releases; best month on every velocity measure (`357da3d`)
- **Builder-to-reviewer chain scan** (2026-09-01, inside #52): all 76 pairs scanned for reviewer-probed sheet names the paired builder never emits. Confirmed breaks: `traceability-matrix`, `test-case-catalog` (probes `Test Case Inventory`, builder emits `Test Cases`), `flexray-config` (probes `Title`, builder emits `Title_Page`). This falsifies the premise on which `docs/chain-contract-audit.md` excludes builder-to-reviewer pairs — #46 should be re-scoped, not closed
- W36 DOCS roll (Fri 2026-09-04): this section; 4 reviewer-side example stubs added — `sysml-block-diagram-checklist-reviewer`, `mbse-system-context-checklist-reviewer`, `mbse-model-architecture-checklist-reviewer`, `mbse-requirements-allocation-checklist-reviewer` — written against the archives, which surfaced the check-count drifts below. STATUS regenerated via `scripts/regen_status.py` — 76/76 paired, 10 fresh / 66 stale / 0 orphan (this commit)

### Known issues _(found these two weeks, deliberately not fixed)_
- **Behaviour change:** `sysml-block-diagram-builder` with no `--input` emits an empty template, not the old five placeholder blocks. Anything scripting the bare command should point at `examples/sysml-block-diagram-builder/sample_input.json`
- Reviewer check-count drift, counted from `check_definitions.py` on 2026-09-04: `sysml-block-diagram-checklist-reviewer` advertises 28 checks / 7 tabs, ships **15 / 3**; `mbse-system-context-checklist-reviewer` advertises "25+", ships **6**; `mbse-model-architecture-checklist-reviewer` advertises "30+", ships **8**; `mbse-requirements-allocation-checklist-reviewer` advertises "28+", ships **6**. Same class as the W33/W34 entries; awaiting the standing-item-9 decision on whether this task authors checks
- `dashboard.py` in the mbse reviewers counts major issues as `NO and obligation == "Shall"` while the checks use `Must`/`Should`, so `REJECTED` can never fire — a `{}` workbook scoring 4 NO reports `CONDITIONAL APPROVAL`. Grep suggests the same split exists elsewhere; repo-wide check queued
- `traceability-matrix` pair non-functional (above); `test-case-catalog` and `flexray-config` each have a one-name sheet mismatch that may be a one-line fix — unconfirmed

---

## [v2026.08.W34] — 2026-08-22

_W34 (2026-08-17 → 2026-08-22). Accumulating since v2026.08.W33 (2026-08-15). Shipped by the Saturday RELEASE run (2026-08-22)._

### Polish
- **Repo-wide chain-contract audit** — W34 target (Tue 2026-08-18, #46) after five consecutive weeks of being carried and never started. Landed only because Monday descoped it to builder-to-builder reads and slotted it *first* instead of last. `scripts/chain_contract_audit.py` + `docs/chain-contract-audit.md` committed; read-only, modifies no `.skill` file. Found one silent BREAK — `fmeda-builder`'s TSC reader — filed as #53 (`5fa7109`)
- **fmeda-builder** — (Wed 2026-08-19, #53): the break the audit found, fixed the day after it was found. The TSC reader expected sheet names the current `tsc-builder` no longer emits, so safety-mechanism allocations were silently absent from the FMEDA worksheet — no exception, no warning, the same failure shape as #43. Repo chain breaks now zero (`ec8fc8e`)
- **autosar-bsw-config-builder** — W34 target (Thu 2026-08-20, #51): first pass on the largest never-polished domain. Gained `examples/sample_input.json` and a real `recalc.py` (restored byte-identical from `autosar-swc-builder`, import verified); the `## Skills inventory` heading corrected. Two defects found and deliberately *not* fixed in the same pass — filed as #54 and #55 rather than expanded into a refactor (`15d4c8b`)

### Docs
- W34 weekly plan published (Mon 2026-08-17) — 3 targets for 3 polish days, down from W33's 4-for-3; #46 descoped and moved to first slot, #51 and #52 opened (`547ced0`)
- W34 DOCS roll (Fri 2026-08-21): `[Unreleased]` filled with the three polish entries above; 2 reviewer-side example stubs added — `fmeda-checklist-reviewer` and `autosar-bsw-config-checklist-reviewer`, the paired reviewers of both builders touched this week. Written against the actual archives rather than the SKILL.md prose, which surfaced two more check-count drifts (below). STATUS regenerated via `scripts/regen_status.py` — 76/76 paired, 16 fresh / 60 stale / 0 orphan (this commit)

### Known issues _(found this week, deliberately not fixed)_
- `fmeda-checklist-reviewer` — `SKILL.md` says "14 + 14 checks" and labels the Confirmation Review tab "14 generic doc-quality checks"; `check_definitions.py` registers **18** `CR` entries, so the true total is **36** (18 CR + 14 FMEDAA + 4 VA), not 28. Found during the Friday stub write
- `autosar-bsw-config-checklist-reviewer` — `SKILL.md` advertises "~30 compliance checks" and the `check_definitions.py` docstring repeats it; the file defines **9** (`C001`–`C009`). Off by more than 3×. Should fold into the #55 batch pass, which already opens this SKILL.md
- Both are the same drift class as `cs-architecture-checklist-reviewer` (42 → 43) and both require a `.skill` zip repack, i.e. POLISH-mode work

### Not done this week
- **#52 (`traceability-matrix-builder`, v&v)** was Thursday's slot and was displaced by #53. That was the right call — #53 was a live chain break found by Tuesday's own audit, and fixing it the next day is the audit paying for itself — but it means W34 landed 3 of 4 items, and the v&v domain is *still* never-polished. #52 should take the first polish slot in W35, the way #46 did in W34

### Carried forward
- Four known defects shipped with v2026.08.W33 remain open and all require a `.skill` zip repack: `cdd-checklist-reviewer` dangling `references/` pointers; `cs-architecture-checklist-reviewer` check count 42 → 43 and 14 → 15 VA; the two interacting `sotif-analysis-checklist-reviewer` probe defects; and the 19 over-length sheet names in `docs/sheet-name-length-audit.md`. See the v2026.08.W33 section of `RELEASES.md`

---

## [v2026.08.W33] — 2026-08-15

_W33 (2026-08-10 → 2026-08-15). Accumulating since v2026.08.W32 (2026-08-08). Shipped by the Saturday RELEASE run (2026-08-15)._

### Fixed
- **STATUS pairing regression** — the Thursday run (`08cf363`) reported `74 strict-paired, 2 naming-mismatch orphans`, contradicting `docs/PAIRING_ALIASES.md`, which states that any STATUS regeneration MUST honor the alias table. Root cause is the one Monday's plan already flagged (`fbcf48e`): *"the STATUS generator is not version-controlled, so its fixes evaporate each run."* Each day re-derived the pairing logic inline, so the alias handling was rebuilt from scratch — and lost. Fixed at the root by committing `scripts/regen_status.py`, which **parses** the alias table out of `docs/PAIRING_ALIASES.md` instead of hard-coding it; a new alias row is now picked up with no code change. Back to 76/76 paired, 0 orphan (this commit)
- **Two builders mis-domained as `safety`** — `msa-gage-rr-builder` and `spc-chart-builder` are Measurement System Analysis (Gage R&R) and SPC tooling, i.e. IATF 16949 quality, not ISO 26262 safety. Nine builders match none of the documented domain prefix rules and were being hand-assigned on each ad-hoc run; all nine are now pinned explicitly in `regen_status.py` so the label is stable, with these two corrected (this commit)

### Docs
- W33 weekly plan published (Mon 2026-08-10) — 4 targets: #48 stub tail, #43 reader rewrite, #46 audit, plus new issue #50 (`cdd-builder`, first diagnostics target since W30) (`fbcf48e`)
- W33 DOCS roll (Fri 2026-08-14): 3 reviewer-side example stubs added for the paired reviewers of skills touched this week — `cdd-checklist-reviewer`, `cs-architecture-checklist-reviewer`, `sotif-analysis-checklist-reviewer`. Written against the actual archives rather than the SKILL.md prose, which surfaced two documentation drifts (below). STATUS regenerated (this commit)

### Polish
- **sotif-analysis-builder** — W33 target (Tue 2026-08-11, #48): tabs 03/07/09 ran 39/35/32 characters against Excel's hard 31-character limit; openpyxl warns but writes through, so shipped workbooks were malformed. Shortened and verified (12 tabs, 0 over-limit). Second-order fix: the reviewer's first-keyword-hit probe was reading tab 03 (function list) as the performance-limitation table, which the rename resolves. Also restored `cdd-builder`'s promised sample fixture and dropped three aspirational `references/` entries. New `docs/sheet-name-length-audit.md` records 19 over-length names across 10 skills (`386191e`)
- **cs-architecture-builder** — W33 target (Wed 2026-08-12, #43): `cs_concept_reader.py` rewritten against the tabs `cs-concept-builder` actually emits, with old names kept as fallbacks; silent empty parse now raises instead of returning nothing. Verified by execution — 0/0/0 before, 2/6/1 after (`b9c8aef`)
- **cs-concept-builder** — (Thu 2026-08-13, #44): re-verified byte-clean; an EOCD-aware rescan found **0** truly corrupt members across all 152 archives, correcting the earlier 13-archive figure, which was scanner false positives on xlsx zip members nested inside the skill bundles (`08cf363`)

### Known issues _(found this week, deliberately not fixed)_
- `cdd-checklist-reviewer` — `SKILL.md` instructs the user to read `references/methodology.md` and `references/cdd_checks.md` and lists both under "Files in this skill"; **neither exists in the archive**, which contains only `SKILL.md` + `scripts/`. Same dangling-reference class as `9b4660e`
- `cs-architecture-checklist-reviewer` — `SKILL.md` claims "42 standard checklist requirements" and "14 verification checks"; `check_definitions.py` defines **15** `va_*` checks, so the true total is **43**. Both the description and the tab table are one short
- `sotif-analysis-checklist-reviewer` — the two interacting probe defects from `386191e` remain open: the insufficiency keyword never matches (count is permanently 0) and the header row is counted as data
- All four require repacking a `.skill` zip, which is POLISH-mode work; queued for next week

## [v2026.08.W32] — 2026-08-08

_W32 (2026-08-03 → 2026-08-08). Accumulating since v2026.08.W31 (2026-08-01). Shipped by the Saturday RELEASE run (2026-08-08)._


### Polish
- **aspice-process-evidence-builder** — W32 target (Tue 2026-08-04, #45): `performance_records`, `quantitative_metrics` and `improvements` input keys now render into tabs 06/07/08; the hard-coded metric list survives only as a named empty-input fallback, and the status JSON gained per-section counts so silently dropped input shows up in smoke tests. Closes the remaining third of #45 (`f00f1b2`)
- **a2l-builder** — W32 target (Wed 2026-08-05, #47): `normalize_members()` added so comma-string `members` no longer report a character count in Record Layouts / Group Hierarchy; `conversion_methods.description` documented in the docstring schema (the worksheet already read it). Both input shapes smoke-tested, 13/13 sheets (`6fab2ce`)
- **safety-program-risk-register-builder** — W32 target (Thu 2026-08-06, #49): first pass on the program-mgmt domain — SKILL.md corrected from a claimed 9 tabs to the true 8, three phantom files dropped from "Files in this skill", the dangling `references/risk_categories.md` pointer removed from the References tab, and a Known-limitations block added for the Risk Trend / Escalation Log empty scaffolds (`9b4660e`)

### Docs
- W32 weekly plan published (Mon 2026-08-03) — no new issues opened; the week carries the five still-open targets #43, #45, #46, #47, #49 (`fcc4c0b`)
- W32 DOCS roll (Fri 2026-08-07): `[Unreleased]` updated with the three W32 polish entries; 3 example README stubs added — `safety-program-risk-register-builder` plus the paired reviewers `a2l-checklist-reviewer` and `aspice-process-evidence-checklist-reviewer`; STATUS regenerated (this commit)

### Release _(this snapshot commit)_
- STATUS.md regenerated (76/76 paired, 12 fresh / 64 stale / 0 orphan)
- RELEASES.md appended with the v2026.08.W32 section
- CHANGELOG `[Unreleased]` rolled into this dated section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

## [v2026.08.W31] — 2026-08-01

_W31 (2026-07-27 → 2026-08-01). Accumulating since v2026.07.W30 (2026-07-25). Shipped by the Saturday RELEASE run (2026-08-01)._

### Polish
- **aspice-gap-analysis-builder** — W31 target (Tue 2026-07-28, #45): the required-but-never-read `assessment.xlsx` CLI arg removed and the docs made honest about what the generator actually consumes (`67c9072`)
- **aspice-improvement-plan-builder** — W31 target (Thu 2026-07-30, #45): same ignored-arg class — the unread `gap_analysis.xlsx` arg removed, docs corrected to match behaviour (`299cdad`)

### Docs
- W31 weekly plan published (Mon 2026-07-27) — targets: cs-concept→cs-architecture reader rewrite (#43, carried), ASPICE ignored-arg/scaffold wiring (#45, carried), repo-wide chain-contract audit (#46, carried), sotif-analysis first pass (#48, new), safety-program-risk-register first pass (#49, new) (`57238bf`)
- W31 DOCS roll (Fri 2026-07-31): `[Unreleased]` updated with W31 entries; 2 reviewer-side example stubs added for the paired reviewers of skills touched this week; STATUS regenerated (`e749617`)

### Release _(this snapshot commit)_
- STATUS.md regenerated (76/76 paired, 10 fresh / 66 stale / 0 orphan)
- RELEASES.md appended with the v2026.08.W31 section
- CHANGELOG `[Unreleased]` rolled into this dated section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

## [v2026.07.W30] — 2026-07-25

_W30 (2026-07-20 → 2026-07-25). Accumulating since v2026.07.W29 (2026-07-18). Shipped by the Saturday RELEASE run (2026-07-25)._

### Polish
- **cs-concept-builder** — W30 target (Tue 2026-07-21, #44): 3,361 trailing NUL bytes stripped from the generator; same pass surfaced 13 more corrupt archives repo-wide (`691f614`)
- **batch NUL-strip, 13 archives** — (Wed 2026-07-22): the 13 corrupt archives found Tuesday cleaned in one batch; 9 generators compile again; xlsx false-positive findings corrected (`8a855cf`)
- **a2l-builder** — W30 target (Thu 2026-07-23, #47): axis-points crash fixed, trigger description strengthened, 13-tab output verified (`661c4bb`)

### Docs
- W30 weekly plan published (Mon 2026-07-20) — targets: cs-concept→cs-architecture chain repair (#43), cs-concept NUL-strip (#44), ASPICE bundle wiring (#45), repo-wide chain-contract audit (#46), a2l polish (#47) (`a0c3d91`)
- W30 DOCS roll (Fri 2026-07-24): `[Unreleased]` updated with W30 entries; 11 example README stubs added for skills touched this week — first reviewer-side stubs, per the touched-this-week rule; STATUS regenerated (this commit)

### Release _(this snapshot commit)_
- STATUS.md regenerated (76/76 paired, 8 fresh / 68 stale; aspice-assessment-builder aged past the 30-day line)
- RELEASES.md appended with the v2026.07.W30 section
- CHANGELOG `[Unreleased]` rolled into this dated section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

## [v2026.07.W29] — 2026-07-18

_W28–W29 (from Mon 2026-07-06). Accumulating since v2026.07.W27 (2026-07-04). The W28 Fri DOCS (2026-07-10) and Sat RELEASE (2026-07-11) runs did not fire, so this section spans two weeks; W28 entries were back-filled by the W29 Fri DOCS roll (2026-07-17). Shipped by the Saturday RELEASE run (2026-07-18)._

### Polish
- **ppap-package-builder** — W28 weekly-target pass (Tue 2026-07-07, #39); embedded fix: over-limit sheet title `05_Element3_Engineering_Approval` (32 chars) renamed to `05_Element3_Eng_Approval`; two chain-break findings logged (tab-09 name >31 chars vs reviewer probe; Element-18 status-tab probe mismatch) (`90288f8`)
- **item-definition-builder** — W28 weekly-target pass (Wed 2026-07-08, #38); resolved via the alias-documentation path — `docs/PAIRING_ALIASES.md` created as the canonical builder↔reviewer alias registry; smoke test green, no `.skill` edits (`26bb385`)
- **aspice-improvement-plan-builder** — W29 polish pass (Wed 2026-07-15); smoke test green (11 tabs), findings logged: required `gap_analysis.xlsx` CLI arg never read (ignored-arg chain-break, same class as aspice-gap-analysis 2026-07-01) and tabs 04–09 are header-only scaffolds; no `.skill` edits (`26ee19a`)
- **aspice-process-evidence-builder** — W29 polish pass (Wed 2026-07-15); clean data point for the ASPICE ignored-arg sweep (input JSON genuinely consumed, tabs 00–05 populated); scaffold-only tabs 06–08 finding logged; no `.skill` edits (`696e912`)
- **cs-architecture-builder** — W29 polish pass (Thu 2026-07-16); smoke test green (12 tabs, 14 CSR allocations), forward chain to reviewer exact (10/10 tab names), but high-severity silent upstream chain-break found: cs-concept-builder output tabs/headers don't match this builder's `cs_concept_reader.py` (a real concept workbook reads back zero CSRs); coordinated fix deferred, no `.skill` edits (`8304e58`)

### Docs
- W28 weekly plan published (Mon 2026-07-06) — targets: item-def pairing (#38), ppap pairing (#39), a2l (#40), sotif (#41), risk-register (#42) (`a431b86`)
- `docs/PAIRING_ALIASES.md` — canonical alias registry for the two name-mismatched pairs: item-definition-builder ↔ item-def-checklist-reviewer, ppap-package-builder ↔ ppap-checklist-reviewer (`26bb385`)
- W29 DOCS roll (Fri 2026-07-17): `[Unreleased]` back-filled with W28 entries (missed 2026-07-10 DOCS run) and updated with W29 polish entries; example README stubs added for ppap-package-builder, aspice-improvement-plan-builder, aspice-process-evidence-builder, and cs-architecture-builder; STATUS regenerated (this commit)

### Release _(this snapshot commit)_
- STATUS.md regenerated (76/76 paired, 4 fresh / 72 stale; dia/fmeda/fsc/hsi/item-definition reclassified other → safety)
- RELEASES.md appended with the v2026.07.W29 section
- CHANGELOG `[Unreleased]` rolled into this dated section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

## [v2026.07.W27] — 2026-07-04

_W27 (Mon 2026-06-29 → Sat 2026-07-04, ISO week 27). Shipped by the Saturday RELEASE run._

### Polish
- **apqp-plan-builder** — W27 polish pass (Tue 2026-06-30); smoke-tested, polish-log entry appended, STATUS regenerated; no `.skill` edits (`2df3374`)
- **item-definition-builder** — W27 polish pass (Wed 2026-07-01); reviewed, naming-mismatch finding logged to skill-polish-log, STATUS regenerated; no `.skill` edits (`01f38c5`)
- **aspice-gap-analysis-builder** — W27 polish pass (Thu 2026-07-02); smoke-tested, ignored-argument finding logged to skill-polish-log, STATUS regenerated; no `.skill` edits (`260c03f`)

### Docs
- W27 weekly plan published (Mon 2026-06-29) — targets: odx, autosar-bsw-config, mbse-context, sysml-state, traceability (`8847b29`)
- June 2026 monthly KPI report published (Wed 2026-07-01) (`35be610`)
- W27 DOCS roll (Fri 2026-07-03): `[Unreleased]` updated with W27 polish + docs entries; example README stubs added for apqp-plan-builder, item-definition-builder, and aspice-gap-analysis-builder; STATUS regenerated (this commit)

### Release _(this snapshot commit)_
- STATUS.md regenerated (76/76 paired, 3 fresh / 73 stale; generated-on date advanced to 2026-07-04)
- RELEASES.md appended with the v2026.07.W27 section
- CHANGELOG `[Unreleased]` rolled into this dated section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

## [v2026.06.W26] — 2026-06-27

_W26 (Mon 2026-06-22 → Sat 2026-06-27, ISO week 26). Shipped by the Saturday RELEASE run._

### Polish
- **5-why-builder** — W26 polish pass (Tue 2026-06-23); example README stub added and polish-log entry appended; no `.skill` edits (`107d490`)
- **communication-matrix-builder** — W26 polish pass (Wed 2026-06-24); trigger phrasing broadened in description, polish-log entry added, STATUS regenerated (`9389a13`)
- **aspice-assessment-builder** — W26 polish pass (Thu 2026-06-25); embedded fix: capability-level rating rule corrected to ISO/IEC 33020; polish-log entry added, STATUS regenerated (`fd66c47`)

### Docs
- W26 weekly plan published (Mon 2026-06-22) — targets: 5-why, aspice-assessment, cs-concept, dia, communication-matrix (`e2162f8`)
- W26 DOCS roll (Fri 2026-06-26): `[Unreleased]` updated with W26 polish entries; example README stubs added for aspice-assessment-builder and communication-matrix-builder; STATUS regenerated (`57dd9ba`)

### Release _(this snapshot commit)_
- STATUS.md regenerated (76/76 paired, 3 fresh / 73 stale; generated-on date advanced to 2026-06-27)
- RELEASES.md appended with the v2026.06.W26 section
- CHANGELOG `[Unreleased]` rolled into this dated section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

## [v2026.06.W25] — 2026-06-20

_Consolidates the unreleased W24 + W25 work; the W24 Saturday release (2026-06-13) did not fire, so these entries ship under the W25 tag. Commit messages carry "W24" labels (one-week drift vs. ISO week 25)._

### Polish
- **sotif-analysis-builder** — W24 #20 review pass (Tue 2026-06-09); polish-log entry added; no `.skill` edits (`a6df28d`)
- **safety-case-builder** — W24 #21 review pass (Wed 2026-06-17); polish-log entry added; no `.skill` edits (`2bd0c19`)
- **control-plan-builder** — polish pass (Thu 2026-06-18); embedded fix: sample-count in JSON example corrected 18 → 15 to match the 15 controlled characteristics; polish-log entry added (`54e2559`)

### Docs
- W24 weekly plan published — targets: classifier extract, sotif, safety-case, control-plan, comm-matrix (`2623e14`)
- W24 triage pass — label refresh, #12 typed, #17 delta confirmed, STATUS regen (`d8b19ef`)
- W25 DOCS roll (Fri 2026-06-19): `[Unreleased]` backfilled with W24 entries; example README stubs added for control-plan-builder and safety-case-builder; STATUS regenerated (this commit)

## [v2026.06.W23] — 2026-06-06

### Polish
- **cs-concept-builder** — W23 #1 polish pass (Tue 2026-06-02); W20 findings re-confirmed against unchanged archive, polish-log appended; no `.skill` edits (description rewrite outside autonomous allowlist) (`5b4b006`)
- **tara-builder** — W23 #2 polish pass (Wed 2026-06-03); new polish-log entry with 1 medium-severity finding (Auto-rating Heuristics internal contradiction) + 3 low-severity items; no `.skill` edits (`22d6409`)
- **fmeda-builder** — W23 #3 polish pass (Thu 2026-06-04); new polish-log entry with 2 medium-severity findings (Classification ladder unreachable branch; SMvDU non-standard acronym) + 3 low-severity items including 100× unit-convention suspicion in JSON example; no `.skill` edits (`d6afa26`)

### Docs
- W23 weekly plan published — targets: cs-concept, aspice-assessment, classify_skill.py extraction (#10), fmeda, tara; carryovers #4 and #5 referenced in place, fresh issues #15 and #16 opened (`3af1f6b`)
- W23 example README stubs added for skills touched this week (cs-concept-builder, tara-builder, fmeda-builder) (this commit)
- W23 CHANGELOG roll: 3 polish entries + 3 docs entries staged under `[Unreleased]` (this commit)
- May 2026 monthly KPI report published — 23 commits, 24 distinct skills touched, 3 weekly releases, 100% paired ratio, 7.9% example coverage; SOTIF domain flagged zero-touch in May (`f8e940f`)
- v2026.06.W23 weekly snapshot tagged; RELEASES.md updated with W23 section; STATUS regenerated

## [v2026.05.W22] — 2026-05-30

### Polish
- **uds-services-builder** — W22 #1 polish pass; small fixes applied in-skill, findings logged to skill-polish-log, STATUS regenerated (`9850780`)
- **dfmea-builder** — W22 #5 polish pass; findings logged to skill-polish-log, STATUS regenerated (`04482aa`)
- **hara-builder** — W22 #2 polish pass; findings logged to skill-polish-log, STATUS regenerated (`ef78172`)

### Docs
- W22 weekly plan published — targets: uds-services, hara, cs-concept, aspice-assessment, dfmea; carryover items from W20/W21 absorbed and 1 new tracking issue (#12) opened (`973c075`)
- W22 example README stubs added for skills touched this week (uds-services-builder, hara-builder, dfmea-builder) (`f9e63f9`)
- STATUS.md regen aliasing applied for `item-definition` ↔ `item-def-checklist-reviewer` and `ppap-package` ↔ `ppap-checklist-reviewer` so suite stays at 100% paired (`f9e63f9`)
- v2026.05.W22 weekly snapshot tagged; RELEASES.md updated with W22 section

## [v2026.05.W21] — 2026-05-23

### Polish
- **autosar-swc-builder** — W21 #8 polish pass; description fix applied, STATUS regenerated (`387bbcd`)
- **dbc-builder** — W21 #7 polish pass; findings logged to skill-polish-log, STATUS regenerated (`76b2fff`)
- **8d-problem-solving-builder** — W21 #6 polish pass; findings logged to skill-polish-log, STATUS regenerated (`e8ff3b2`)

### Docs
- W21 weekly plan published — targets: 8d carryover, dbc, autosar-swc, uds-services, classifier-freeze (`3320c23`)
- CHANGELOG.md introduced; `examples/<skill>/README.md` stubs added for skills touched in W21 (`1a5ca80`)
- v2026.05.W21 weekly snapshot tagged; RELEASES.md updated

## [v2026.05.W20] — 2026-05-16

### Polish
- **hara-builder**, **cs-concept-builder**, **aspice-assessment-builder** — W20 polish passes; findings logged to skill-polish-log

### Docs
- W20 weekly plan published; RELEASES.md created; first autonomous-cadence weekly snapshot tagged
