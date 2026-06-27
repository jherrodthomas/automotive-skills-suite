# Automotive Skills Suite — Releases

Weekly snapshots of `github.com/jherrodthomas/automotive-skills-suite`. Tags are lightweight; the human clicks Publish on GitHub after reviewing this file.

---

## v2026.05.W20 — 2026-05-16

ISO week 20 (2026-05-11 → 2026-05-16). First weekly snapshot of the autonomous run cadence.

### Highlights

- Three builders moved from "shipped" to "reviewed against the polish playbook": `hara-builder`, `cs-concept-builder`, `aspice-assessment-builder`. Each now carries a dated entry in `docs/skill-polish-log/` with severity-rated findings.
- Weekly target file `docs/weekly/WEEK-2026-W20.md` opened the loop on Monday and the four GitHub issues it spawned are now mostly serviced; one (`8d-problem-solving-builder`) carries forward to W21.
- Suite still at 76 builder + 76 reviewer pairs, 100% paired (`item-definition-builder` ↔ `item-def-checklist-reviewer` and `ppap-package-builder` ↔ `ppap-checklist-reviewer` are alias pairs and now reflected correctly in STATUS).

### Changes this week

**plan**
- `16df7a5` auto(plan): W20 targets — hara, cs-concept, aspice-assessment, 8d

**polish**
- `5b6ad4e` auto(polish): regen STATUS.md and log W20 hara-builder polish findings
- `49fd9a9` auto(polish): regen STATUS.md and log W20 cs-concept-builder polish findings
- `c4fad8a` auto(polish): W20 #5 aspice-assessment-builder pass, regen STATUS.md

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (no flag changes vs. prior run)
- RELEASES.md created
- `docs/AUTONOMOUS_LOG.md` updated with RELEASE-mode entry

### Skills inventory

- Builders: 76
- Reviewers: 76
- Paired: 76/76 (100.0%)
- Domain spread: safety=12, quality=8, comms=8, program-mgmt=6, cyber=6, autosar=5, diagnostics=5, v&v=5, aspice=4, sysml=4, mbse=3, sotif=3, calibration=3, other=4

### Open issues at snapshot

- #6 W20 polish target: 8d-problem-solving-builder.skill _(carries to W21)_
- #5 W20 polish target: aspice-assessment-builder.skill _(serviced this week, awaiting close-out)_
- #4 W20 polish target: cs-concept-builder.skill _(serviced this week, awaiting close-out)_
- #3 W20 polish target: hara-builder.skill _(serviced this week, awaiting close-out)_
- #2 goodd _(needs human triage — non-conforming title, no domain signal)_

### Compare

[`3c69553...v2026.05.W20`](https://github.com/jherrodthomas/automotive-skills-suite/compare/3c69553...v2026.05.W20)

### Notes for the human

Click Publish on the v2026.05.W20 tag on GitHub once you've skimmed this section. Issue #2 (`goodd`) is the one item Sunday's TRIAGE pass should probably still escalate to you — autonomous run is intentionally not labeling or commenting it because confidence is well below 80%.

---

## v2026.05.W21 — 2026-05-23

ISO week 21 (2026-05-18 → 2026-05-23). Second weekly snapshot — W21 polish cycle closed; CHANGELOG and the first `examples/` doc stubs landed.

### Highlights

- W21 polish ran the full Tue/Wed/Thu cadence over `8d-problem-solving-builder`, `dbc-builder`, and `autosar-swc-builder`; the autosar-swc pass shipped an actual in-allowlist description fix, the other two produced severity-rated polish-log findings held for human review.
- `CHANGELOG.md` introduced and the first `examples/<skill>/README.md` stubs added — the repo now has a docs spine beyond STATUS.
- Suite steady at 76 builder + 76 reviewer pairs, 100% paired; no flag changes vs. W20.

### Changes this week

**plan**
- `3320c23` auto(plan): W21 targets — 8d carryover, dbc, autosar-swc, uds-services, classifier-freeze

**polish**
- `e8ff3b2` auto(polish): W21 #6 8d-problem-solving-builder pass, regen STATUS.md
- `76b2fff` auto(polish): W21 #7 dbc-builder pass, regen STATUS.md
- `387bbcd` auto(polish): W21 #8 autosar-swc-builder pass — description fix applied, STATUS regen

**docs**
- `1a5ca80` auto(docs): add CHANGELOG, W21 example README stubs, regen STATUS

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (no flag changes vs. prior run)
- RELEASES.md updated with the W21 section
- CHANGELOG.md `[Unreleased]` rolled into a dated `[v2026.05.W21]` section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

### Skills inventory

- Builders: **76** · Reviewers: **76** · Paired ratio: **100.0%** (76/76)
- Domain spread: safety 14, quality 8, comms 8, program-mgmt 6, cyber 6, autosar 5, diagnostics 5, v&v 5, aspice 4, sysml 4, calibration 3, mbse 3, sotif 3, other 2

### Compare

https://github.com/jherrodthomas/automotive-skills-suite/compare/v2026.05.W20...v2026.05.W21

### Open items for the human

- 10 issues open. Issue **#2** ("goodd", empty body) has stayed `needs-triage` across multiple runs — needs a human decision (likely close as spam/invalid).
- Issue **#11** ("packaging utility for Claude-compatible skill exports") is still unlabeled — Sunday TRIAGE should label it.
- W21 polish target **#9** (`uds-services-builder`) and tooling target **#10** (classifier freeze) were not serviced this week — both carry to W22.

---

## v2026.05.W22 — 2026-05-30

ISO week 22 (2026-05-25 → 2026-05-30). Third weekly snapshot — W22 polish cycle closed, alias map landed in the STATUS regen helper, RELEASE cadence now three weeks running.

### Highlights

- W22 polish ran the Tue/Wed/Thu cadence over `uds-services-builder`, `dfmea-builder`, and `hara-builder`; the uds-services pass shipped an in-allowlist description rewrite (DIDs, RIDs, SecurityAccess, NRC, P2/P2*/S3 timing, three session types, 0x10-0x86 service range, casual phrasings, sibling redirects) and finally cleared the W21-era classifier blind spot. The other two produced severity-rated polish-log entries held for human review.
- The reviewer-name alias map (`item-definition` ↔ `item-def`, `ppap-package` ↔ `ppap`) was promoted from inline override into the STATUS regen helper on Fri DOCS, so the 100% paired headline is now data-driven rather than hand-patched per run.
- Three new `examples/<skill>/README.md` stubs landed (uds-services, hara, dfmea) bringing the docs spine to seven seeded skills. CHANGELOG groups today's W22 work cleanly into Polish (3) and Docs (1 plan + 1 docs commit).
- Suite steady at 76 builder + 76 reviewer pairs, 100% paired; no flag changes vs. W21.

### Changes this week

**plan**
- `973c075` auto(plan): W22 targets — uds, hara, cs-concept, aspice-assessment, dfmea

**polish**
- `9850780` auto(polish): W22 #1 uds-services-builder pass, regen STATUS
- `04482aa` auto(polish): W22 #5 dfmea-builder pass, regen STATUS, journal
- `ef78172` auto(polish): W22 #2 hara-builder pass, regen STATUS, journal

**docs**
- `f9e63f9` auto(docs): W22 changelog roll-up, 3 example stubs, STATUS regen

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (no flag changes vs. prior run; 76/76 paired)
- RELEASES.md updated with the W22 section
- CHANGELOG.md `[Unreleased]` rolled into a dated `[v2026.05.W22]` section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

### Skills inventory

- Builders: **76** · Reviewers: **76** · Paired ratio: **100.0%** (76/76)
- Domain spread: safety 13, other 9, comms 8, cyber 6, autosar 5, diagnostics 5, quality 5, v&v 5, aspice 4, sysml 4, calibration 3, mbse 3, program-mgmt 3, sotif 3

### Compare

https://github.com/jherrodthomas/automotive-skills-suite/compare/v2026.05.W21...v2026.05.W22

### Open items for the human

- 10 issues open. The five W22-touched targets (#3 hara, #4 cs-concept, #5 aspice-assessment, #9 uds-services, #12 dfmea) now all carry W22 polish-log evidence — recommend a sweep to close the ones the human considers done.
- Issue **#10** (classifier freeze) is now 7+ runs old; this week's Fri DOCS pass got the alias map into the helper but the helper is still inline-Python in each commit. Mon W23 PLAN should allocate a tooling slot to extract `scripts/classify_skill.py`.
- Issue **#11** (skill packaging utility) and the W22 carryover targets (cs-concept, aspice-assessment, which only have W20-dated polish-log entries) are the natural W23 candidates if the human wants visible motion next week.

---

## v2026.06.W23 — 2026-06-06

ISO week 23 (2026-06-01 → 2026-06-06). Fourth weekly snapshot — first of June. Tag scheme ruling defaulted to ISO-absolute (`W23`, continuing the W20/W21/W22 series) after four unanswered flags; per-month spelling can be re-cut from the same SHA if a maintainer prefers.

### Highlights

- W23 polish cycle completed its three passes: `cs-concept-builder` (W20 findings re-confirmed), `tara-builder` (1 medium finding — Auto-rating Heuristics internal contradiction), and `fmeda-builder` (2 medium findings — Classification ladder unreachable branch; `SMvDU` non-standard acronym — plus a suspected 100× unit-convention discrepancy in the JSON example). **None of these were applied** — they are polish-log findings carried into the W24 maintainer backlog, not fixes in this snapshot.
- May 2026 monthly KPI report published (23 commits, 24 distinct skills touched, 3 weekly releases, 100% paired ratio; SOTIF flagged as the only zero-touch domain in May).
- Example-stub coverage rose from 7.9% (6/76) to 11.8% (9/76) with new stubs for cs-concept, tara, and fmeda.
- Suite steady at 76 builder + 76 reviewer pairs, 100% paired; canonical domain classifier (Tue 2026-06-02 explicit map) re-applied, spread stable at safety=15 / quality=10.

### Changes this week

**plan**
- `3af1f6b` auto(plan): W23 targets — cs-concept, aspice-assessment, classifier, fmeda, tara

**polish**
- `5b4b006` auto(polish): W23 #1 cs-concept-builder pass, regen STATUS, journal
- `22d6409` auto(polish): W23 #2 tara-builder pass, regen STATUS, journal
- `d6afa26` auto(polish): W23 #3 fmeda-builder pass, regen STATUS, journal

**docs**
- `f8e940f` auto(monthly): KPI report for May 2026
- `c21a84a` auto(docs): W23 changelog roll, 3 example stubs, STATUS regen

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (76/76 paired; canonical spread; no flag changes vs. Fri)
- RELEASES.md updated with the W23 section
- CHANGELOG.md `[Unreleased]` rolled into a dated `[v2026.06.W23]` section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

### Skills inventory

- Builders: **76** · Reviewers: **76** · Paired ratio: **100.0%** (76/76)
- Domain spread: safety 15, quality 10, comms 8, cyber 6, autosar 5, diagnostics 5, program-mgmt 5, v&v 5, aspice 4, sysml 4, calibration 3, mbse 3, sotif 3

### Compare

https://github.com/jherrodthomas/automotive-skills-suite/compare/v2026.05.W22...v2026.06.W23

### Open items for the human

- 12 issues open. The W20-era carryovers (#4 cs-concept, #5 aspice-assessment) have carried ready-to-apply rewrites in the polish log for 3+ weeks — recommend converting both to maintainer PRs in W24 PLAN rather than re-queuing as polish targets.
- The fmeda medium findings (Classification ladder unreachable branch; SMvDU acronym needing an ISO 26262-5 Annex B source-check; 100× `distribution_pct` unit suspicion) are the highest-leverage maintainer items from W23 — see `docs/skill-polish-log/fmeda-builder.md`.
- Issue **#10** (classifier extraction to `scripts/classify_skill.py`) is now 8 consecutive runs of hand-maintained inline Python — the explicit map grew again this week (`item-definition` → safety).
- Issue count dropped 13 → 12 between Thu and Fri without autonomous action; presumed human/spam-filter close of #17 — Sun TRIAGE will confirm.

## v2026.06.W25 — 2026-06-20

ISO week 25 (2026-06-15 → 2026-06-20). This snapshot consolidates two weeks of work: the **W24 Saturday release (2026-06-13) did not execute**, so everything committed since the last tag (`v2026.06.W23`, 2026-06-06) ships here — the W24 plan/triage/polish plus the W25 docs roll. Internal commit messages carry "W24" labels (a one-week drift between the human's week labels and ISO week numbers); the tag follows the established ISO-week-of-year convention.

### Highlights

- Three polish passes landed against `sotif-analysis-builder`, `safety-case-builder`, and `control-plan-builder`. Only `control-plan-builder` carried an embedded `.skill` fix (JSON example sample-count corrected 18 → 15 to match its 15 controlled characteristics); the other two are polish-log entries with no `.skill` edits.
- The W24 plan opened the loop on five targets (classifier extraction, sotif, safety-case, control-plan, comm-matrix); a triage pass typed #12 and confirmed the #17 delta. All issues have since been serviced or closed — the tracker is at **0 open** at snapshot.
- Suite holds at 76 builder + 76 reviewer pairs, 100% paired. No orphans, no new skills this period.

### Changes this week

**plan**
- `2623e14` auto(plan): W24 targets — classifier extract, sotif, safety-case, control-plan, comm-matrix

**polish**
- `a6df28d` auto(polish): W24 #20 sotif-analysis-builder review pass, regen STATUS, journal
- `2bd0c19` auto(polish): W24 #21 safety-case-builder review pass, regen STATUS, journal
- `54e2559` auto(polish): control-plan-builder pass, fix sample count 18->15, regen STATUS, journal

**triage**
- `d8b19ef` auto(triage): label refresh, #12 typed, #17 delta confirmed, STATUS regen

**docs**
- `6051a5d` auto(docs): backfill W24 changelog, add 2 example stubs, regen STATUS

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (76/76 paired; no flag changes vs. Fri — only the generated-on date advanced)
- RELEASES.md updated with this W25 section
- CHANGELOG.md `[Unreleased]` rolled into a dated `[v2026.06.W25]` section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

### Skills inventory

- Builders: **76** · Reviewers: **76** · Paired ratio: **100.0%** (76/76)
- Domain spread: safety 17, quality 10, comms 8, cyber 6, autosar 5, diagnostics 5, v&v 5, aspice 4, sysml 4, calibration 3, mbse 3, program-mgmt 3, sotif 3

### Compare

https://github.com/jherrodthomas/automotive-skills-suite/compare/v2026.06.W23...v2026.06.W25

### Open items for the human

- **Missed W24 release** — the 2026-06-13 Saturday run did not fire; W24 work is captured here rather than under its own tag. If a continuous tag history matters, a retroactive `v2026.06.W24` could be cut at `6051a5d`, but that is a manual call left to the maintainer.
- **Week-label drift** — commit messages and weekly docs use "W24" for work done in ISO week 25. Worth deciding whether to align the human labels to ISO weeks or document the offset, so future tags don't look like they skip a number (W23 → W25).
- **Issue #10** (classifier extraction to `scripts/classify_skill.py`) remains open conceptually — STATUS regen still relies on parsing the prior STATUS for the domain map rather than a committed classifier. No `feat:` work was scheduled this period.


## v2026.06.W26 — 2026-06-27

ISO week 26 (2026-06-22 → 2026-06-27). Saturday RELEASE run; W25 release fired last week, so this is a normal single-week snapshot with no backfill drift.

### Highlights

- A full W26 polish rotation landed: `5-why-builder`, `communication-matrix-builder`, and `aspice-assessment-builder` each took a review pass. Only `aspice-assessment-builder` carried an embedded `.skill` fix (capability-level rating rule corrected to ISO/IEC 33020); the other two are description/log touches.
- Week cadence is back in sync — commit labels, weekly docs, and the ISO week all read "W26" this period, closing the W23→W25 numbering drift called out in the W25 release notes.
- Suite holds at 76 builder + 76 reviewer pairs, 100% paired. No orphans and no new skills this period; the tracker is at **0 open** at snapshot.

### Changes this week

**plan**
- `e2162f8` auto(plan): W26 targets — 5-why, aspice-assessment, cs-concept, dia, comm-matrix

**polish**
- `107d490` auto(polish): 5-why-builder pass — add example stub, polish log, regen STATUS
- `9389a13` auto(polish): broaden communication-matrix-builder trigger, log, regen STATUS
- `fd66c47` auto(polish): fix aspice-assessment-builder CL rating rule to ISO/IEC 33020

**docs**
- `57dd9ba` auto(docs): W26 changelog roll, 2 example stubs, STATUS regen

**docs / release** _(this snapshot commit)_
- STATUS.md refreshed (76/76 paired; no flag changes vs. Fri — only the generated-on date advanced)
- RELEASES.md updated with this W26 section
- CHANGELOG.md `[Unreleased]` rolled into a dated `[v2026.06.W26]` section
- `docs/AUTONOMOUS_LOG.md` updated with the RELEASE-mode entry

### Skills inventory

- Builders: **76** · Reviewers: **76** · Paired ratio: **100.0%** (76/76)
- Domain spread: safety 17, quality 10, comms 8, cyber 6, autosar 5, diagnostics 5, v&v 5, aspice 4, sysml 4, calibration 3, program-mgmt 3, mbse 3, sotif 3

### Compare

https://github.com/jherrodthomas/automotive-skills-suite/compare/v2026.06.W25...v2026.06.W26
