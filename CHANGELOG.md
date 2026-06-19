# Changelog

All notable changes to the Automotive Skills Suite. Maintained by the autonomous
daily standup. Entries are grouped by intent (feat / fix / polish / docs) and move
from `[Unreleased]` into a dated section at each weekly release.

## [Unreleased]

_Accumulating since v2026.06.W23 (2026-06-06). Note: the W24 Friday docs run (2026-06-12) did not execute, so the W24 polish/triage/plan entries below were backfilled into `[Unreleased]` during the 2026-06-19 DOCS run to keep them from being lost at the next release._

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
