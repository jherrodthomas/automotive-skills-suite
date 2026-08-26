# autosar mis-generated template batch (issue #55)

Batch polish record. This file covers **5 archives across 3 autosar pairs** rather than one skill,
because the defect is one mis-generated template batch, not five independent bugs. The sixth file in
the batch, `autosar-bsw-config-builder.skill`, was fixed during the W34 #51 pass.

Files covered:

- `autosar-adaptive-app-builder.skill` / `autosar-adaptive-app-checklist-reviewer.skill`
- `autosar-rte-mapping-builder.skill` / `autosar-rte-mapping-checklist-reviewer.skill`
- `autosar-bsw-config-checklist-reviewer.skill`

---

## 2026-08-26 — batch fix, severity LOW (latent + consistency)

Taken from POLISH priority rule (1): `#55` is labelled `skill-bug` and is the most recently updated
open bug on the board. It was also the named Wednesday slot in the W35 plan, so rule (1) and the
plan agreed and no re-derivation was needed.

### What's good

- The defect was already fully characterised in #55 before this pass started. Affected file list,
  byte counts, and the exact three deviations were all correct as written — the scan reproduced them
  with no surprises. A DoD this specific is why the fix took one pass.
- The three deviations are genuinely orthogonal to skill logic. None of the five archives needed a
  single line of skill-specific reasoning: `recalc.py` is a byte-identical copy, and the heading is a
  string replacement. Low-risk by construction.
- All 147 non-stub copies of `recalc.py` are **byte-identical** (one sha256 across 147 files), so
  "restore the canonical file" was unambiguous — there was no variant to choose between.

### What was fixed

**Deviation 1 — stub `recalc.py`.** Restored the canonical 5,782-byte file into all 5 archives.
Baseline was `39 bytes` (`# Placeholder for recalc functionality`).

- Before: `{5782: 147, 39: 5}`
- After: `{5782: 152}`, and **1 distinct sha256 across all 152**
  (`cf419d15e02965aeaec17773c05b61303f7f520f67fa2696547e7da07502eac7`).

**Deviation 2 — `## Skills inventory` heading.** Replaced with `## When to use` in all 5 `SKILL.md`
files, matching the heading `autosar-bsw-config-builder` was corrected to in W34. In every case the
section body was trigger guidance ("Use this skill whenever the user mentions…"), so the rename made
the heading describe its own contents; **no body text was altered**. Heading offenders repo-wide:
5 → **0 of 152**. All five now carry the peer heading set
`# <name>` · `## When to use` · `## How to use` · `## Output`.

### What was NOT fixed, and why — deviation 3 corrected

**#55 is wrong about `scripts/__init__.py`, and this is worth recording.** The issue calls it "the
fingerprint that confirms the shared origin" and states the same 6 carry it "that peer skills do
not." The repo-wide scan says otherwise — **9 skills carry `scripts/__init__.py`**: the 6 batch
files plus `cs-architecture-builder`, `hsi-builder`, and `hw-architecture-builder`.

So it is not a fingerprint and it is not unique to the batch. Decision: **leave it in place.** It is
an empty package marker, it is harmless, it is not exclusive to the defective batch, and removing it
would touch three unrelated archives to enforce a convention no one has stated. Removing it from
only the 5 would also make the batch *less* consistent with three peers that keep it. DoD item 3
asked for a decision either way; this is the decision, recorded rather than silently skipped.

### Verification

- Repo-wide re-scan after repack: `recalc` sizes `{5782: 152}`, 1 distinct hash, heading offenders
  `[]`, **0 broken archives** (`ZipFile.testzip()` clean on all 152).
- Every `.py` member of each of the 5 touched archives compiled with `py_compile` from the repacked
  file — 26 modules total, no failures. This covers `recalc.py` plus each skill's own
  `generate_*.py`, `*_probe.py`, `check_definitions.py`, and `dashboard.py`.
- Repack preserved original member order, names, timestamps and permission bits; only the two
  intended members differ per archive. Archive growth (~1.9 KB each) is entirely the real
  `recalc.py` replacing the stub.
- No `__pycache__` leaked into the repack — verification compiled into a scratch tempdir outside the
  archive tree, deliberately, after last run's contamination.

### Severity

**LOW**, as #55 itself argued and this pass confirms: none of the three builders emits formula cells,
so the stub `recalc.py` had nothing to recalculate and no user-visible failure existed. This closes a
latent defect and a consistency gap. Stating that plainly is more useful than dressing it up.

### Follow-ups

- `#55` DoD is met on all 5 items. Commented in-thread; not closed (issues are never closed
  autonomously).
- The 152/152 recalc invariant and the 0/152 heading invariant are now both true and are cheap to
  assert. Worth folding into `scripts/` as a lint so a future mis-generated batch is caught at
  commit time rather than five weeks later by a polish pass.
- `scripts/__init__.py` presence is inconsistent across the repo (9 of 152) but benign — noted here
  so the next person who notices it does not re-open it as a defect.
- Still open from #54: the `Bus Interfaces` tab in `autosar-bsw-config-builder` is written but
  audited by no reviewer check.
