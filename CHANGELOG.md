# Changelog

All notable changes to the Automotive Skills Suite. Maintained by the autonomous
daily standup. Entries are grouped by intent (feat / fix / polish / docs) and move
from `[Unreleased]` into a dated section at each weekly release.

## [Unreleased]

_Accumulating since v2026.07.W29 (2026-07-18). Ships at the next Saturday RELEASE run._

### Polish
- **cs-concept-builder** — W30 target (Tue 2026-07-21, #44): 3,361 trailing NUL bytes stripped from the generator; same pass surfaced 13 more corrupt archives repo-wide (`691f614`)
- **batch NUL-strip, 13 archives** — (Wed 2026-07-22): the 13 corrupt archives found Tuesday cleaned in one batch; 9 generators compile again; xlsx false-positive findings corrected (`8a855cf`)
- **a2l-builder** — W30 target (Thu 2026-07-23, #47): axis-points crash fixed, trigger description strengthened, 13-tab output verified (`661c4bb`)

### Docs
- W30 weekly plan published (Mon 2026-07-20) — targets: cs-concept→cs-architecture chain repair (#43), cs-concept NUL-strip (#44), ASPICE bundle wiring (#45), repo-wide chain-contract audit (#46), a2l polish (#47) (`a0c3d91`)
- W30 DOCS roll (Fri 2026-07-24): `[Unreleased]` updated with W30 entries; 11 example README stubs added for skills touched this week — first reviewer-side stubs, per the touched-this-week rule; STATUS regenerated (this commit)

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
