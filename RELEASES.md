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
