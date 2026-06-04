# fmeda-builder polish log

_Polish target for W23 (issue [#15](https://github.com/jherrodthomas/automotive-skills-suite/issues/15)). Reviewer: autonomous daily-standup task._

---

## 2026-06-04 — first POLISH pass

**Mode:** POLISH (Thursday — third POLISH day of W23)
**File reviewed:** `skills/fmeda-builder.skill` (ZIP archive; SKILL.md is 9,673 bytes / ~155 lines; 10 files total in archive).
**DoD recap (from `docs/weekly/WEEK-2026-W23.md`):**
confirm the description's "13-tab workbook" claim matches the actual generator tab list,
spot-check the SPFM / LFM / PMHF target numbers against ISO 26262-5 Table 6,
and audit the "Key formulas" section in SKILL.md for internal consistency.

### DoD verdict

| DoD check | Result |
|---|---|
| 13-tab claim matches generator schema | **PASS** (cross-check vs. Output structure table; rows 00–12 enumerated) |
| SPFM / LFM / PMHF target numbers vs. ISO 26262-5 Table 6 | **PASS** (90/97/99 for SPFM B/C/D, 60/80/90 for LFM B/C/D, 100/100/10 FIT for PMHF B/C/D — all match standard) |
| "Key formulas" section internal consistency | **FAIL** — Classification ladder has a logically unreachable branch (see finding #1) |

### What's good

- **Description is well-shaped.** 720 / 1024 chars, names every formal trigger phrase
  a user would type ("FMEDA", "SPFM", "LFM", "PMHF", "ISO 26262-5", "diagnostic
  coverage", "safety mechanisms"), and closes with the suite-standard "Always use this
  skill instead of producing a freeform FMEDA in chat." line. No version-number gap of
  the kind that bit `aspice-assessment-builder` (#5) and `cs-concept-builder` (#4) —
  there's only one ISO 26262 revision relevant here (2018).
- **Frontmatter is clean.** Both required keys present (`name`, `description`); YAML
  parses without complaint; no drift fields.
- **ASIL targets are right.** Intro paragraph and PMHF pitfall section agree, and both
  match ISO 26262-5:2018 Table 6 (SPFM ≥90/97/99% for ASIL B/C/D; LFM ≥60/80/90% for
  B/C/D; PMHF ≤100/100/10 FIT for B/C/D). This is the math users would most plausibly
  mis-remember, so getting it locked in two places is a real strength.
- **13-tab list is exact.** The Output Structure table (00 Title Page → 12 References)
  enumerates 13 rows and the Step 4 review checklist references tabs `03` through `11`
  by name — every reference resolves to a tab in the table. No drift between the
  description's "13-tab" claim and the body.
- **Workflow ordering is correct.** Step 1 (BOM JSON) → Step 2 (read references, with
  the right three files named in the right order) → Step 3 (run generator + recalc)
  → Step 4 (analyst review, tab-by-tab). The Step 2 instruction to read the bundled
  references is the pattern `aspice-assessment-builder` is *missing* and `fmeda-builder`
  gets right.
- **Pre-requisites are explicit.** "A TSC workbook (produced by tsc-builder)" and
  "A HW BOM JSON" — names the upstream skill by ID, not by hand-wave. Makes the chain
  dependency auditable.
- **Common pitfalls section is mentor-quality content.** Items #1 (FIT sourcing),
  #2 (DC% evidence), #3 (residual fault accounting), and #4 (PMHF units) are exactly
  the four mistakes a junior FuSa engineer makes in their first FMEDA. This is not
  filler — it's the right content for an audit-grade builder.

### What to fix

1. **MEDIUM — Classification ladder in "Key formulas" has an unreachable branch.**
   The current text reads (paraphrased):

   ```
   If not safety-related → S
   If allocated mechanism with DC% and DC% = 100 → S
   Else if allocated mechanism → check DC%:
       If DC% < 100 → RF
       If no mechanism or DC% = 0 → SPF
       If allocated mechanism but covers multiple points → MPF_D or MPF_L
   ```

   The inner "If no mechanism or DC% = 0 → SPF" branch sits inside an "Else if
   allocated mechanism" block — so the "no mechanism" disjunct is logically
   unreachable from that position. Two fixes are possible: (a) flatten the ladder so
   "no mechanism → SPF" is a sibling branch of "allocated mechanism with DC% = 100 →
   S", or (b) restate as a decision table. Either way, the current nesting cannot
   be evaluated as written. Severity: **medium** (math content; a careful analyst
   reading the formula spec literally will get the wrong classification for an
   unmechanized failure mode).

2. **MEDIUM — "SMvDU (Safe Fault Metric)" acronym in pitfall #5 is not a standard
   ISO 26262 term.** The Common Pitfalls section closes with:
   > "This is why LFM has separate targets — it's about SMvDU (Safe Fault Metric)
   > paired with the architecture's diagnostic coverage of two-point faults."

   Neither ISO 26262-5:2018 nor the standard FuSa vocabulary (Annex B) defines
   "SMvDU". The closest standard terms are λ_S (safe fault rate), MPF_DP (multipoint
   fault, detectable/perceivable), and λ_S,LF (safe latent fault contribution). The
   parenthetical "(Safe Fault Metric)" suggests the author meant either the Safe
   Fault contribution to LFM, or possibly conflated MPF_L (latent multipoint) with
   a Safe-Fault-style ratio. Severity: **medium** (a reader will Google "SMvDU"
   and find nothing). Fix requires a real source check, not a typo correction.

3. **LOW — SPFM and LFM intro phrasings invert the metric direction.** Intro reads:
   > "**SPFM** ... Coverage of *undetected* failure modes. Target: 90% (ASIL B), ..."
   > "**LFM** ... Coverage of *latent* (undetected two-point) failures. Target: 60% ..."

   By ISO 26262-5 definition, SPFM = 1 − Σ(λ_SPF + λ_RF) / Σ(λ_safety_related) and
   LFM = 1 − Σ(λ_MPF,L) / (Σ(λ_safety) − Σ(λ_SPF + λ_RF)). Both metrics measure the
   fraction of safety-related faults that ARE handled (safe or detected), not the
   fraction undetected. The intro's "Coverage of *undetected*" is the inverse of
   what the formula actually computes — a target of 99% SPFM means 99% of single-
   point and residual fault rate is COVERED, not undetected. Severity: **low**
   (the targets themselves are right and Step 4 later describes metrics correctly,
   but the one-line definitions at the top of SKILL.md will mislead a reader
   skimming the intro).

4. **LOW — `failure_mode_overrides` JSON example uses `distribution_pct: 0.3`.**
   In the BOM JSON example the override row says `"distribution_pct": 0.3` but the
   "Key formulas" section says `λ = FIT × (distribution % / 100)`. So a
   distribution of 0.3 in the JSON would mean 0.3% of failures — almost certainly
   not the author's intent (a stuck-at fault is typically 30–40% of an MCU's
   failure mode budget, not 0.3%). Either the JSON example should read `30` or the
   formula should read `× distribution_pct` (without the `/100`). Severity: **low**
   (the example is documentary, not run-tested, but an analyst cloning the example
   will produce an under-estimate by 100×). Fix needs a maintainer eyeball to
   confirm the intended unit convention before flipping either side.

5. **LOW (obs) — Step 4 instruction "_When done, save + re-run `recalc.py`_" lacks
   the same explicit error-checking the rest of the workflow does.** Other suite
   skills (e.g. `tsc-builder`) name `#REF!` and `#DIV/0!` as the specific tokens to
   grep for; this skill says "confirm all formulas evaluate and no #REF/#DIV/0
   errors remain." The phrasing is close but the `!` punctuation is dropped, which
   matters for a CTRL-F. Severity: **low (obs)**.

### Suggested edits (NOT applied this run)

The autonomous-edit allowlist for the daily-standup task is narrow — typo /
over-length description / missing-required-frontmatter-field. **None of the five
findings match that list:**

- #1 (Classification ladder) needs a structural rewrite of a multi-line spec
  block, not a surgical edit. Requires a 5–10-line replacement with re-checked
  precedence; better to do this with a human eyeball.
- #2 (SMvDU acronym) needs source research to identify the right replacement term,
  not a substitution. Could be deletion, could be a re-naming to λ_S,LF — either
  way it's a content decision.
- #3 (SPFM/LFM intro inversion) is a two-sentence rewrite touching the
  most-skimmed part of the SKILL.md. Worth doing carefully.
- #4 (distribution_pct unit) requires deciding which side (JSON or formula) is the
  source of truth, then changing the other to match — and possibly re-checking
  the generator script against whichever side is kept. Not a surgical edit.
- #5 is a `!`-punctuation tidy. Borderline allowlist material but I'm declining
  on the conservative reading.

**No code edits committed today.** Issue #15 stays open with this log linked from
the journal entry.

Proposed minimal rewrite of the SPFM / LFM intro lines (for human review):

```
- **SPFM** (Single-Point Failure Metric): Fraction of safety-related fault rate
  that is *not* uncovered single-point or residual. Target: ≥90% (ASIL B),
  ≥97% (C), ≥99% (D).
- **LFM** (Latent Fault Metric): Fraction of remaining safety-related fault rate
  (after SPFM) that is *not* uncovered latent multipoint. Target: ≥60% (B),
  ≥80% (C), ≥90% (D).
- **PMHF** (Probabilistic Metric for Hardware Failures): Raw uncovered failure
  rate (λ_SPF + λ_RF), in FIT. Target: ≤100 FIT (B/C), ≤10 FIT (D).
```

Proposed flattened Classification ladder:

```
If not safety-related                                  → S
Else if allocated mechanism AND DC% = 100              → S (safe due to detection)
Else if allocated mechanism AND covers multipoint:
    If second-fault detection in place                 → MPF_D
    Else                                               → MPF_L
Else if allocated mechanism AND DC% < 100              → RF
Else (no mechanism OR DC% = 0)                         → SPF
```

### Other observations (not fixes, just notes for future passes)

- The skill bundles three references (`methodology.md`, `failure_mode_libraries.md`,
  `metrics_targets.md`) totaling ~23.6 KB — substantial mentor content. Step 2 of
  the workflow correctly tells Claude to read them. This is the right pattern; it
  is what `aspice-assessment-builder` is missing.
- Generator script is 20.5 KB. Did not deep-audit script content this pass; the
  STATUS regen + this log filled the slot. Worth a script-level pass on a future
  POLISH day, particularly to confirm that the actual classification logic in
  `generate_fmeda.py` matches whichever flattened ladder we land on for finding #1.
- No `.placeholder` cruft in this archive (unlike `aspice-assessment-builder`).
  Archive is clean.
- The `examples/sample_fmeda_bom_esc.json` example is a real ESC (Electronic
  Stability Control) BOM, not a toy. Likely valuable as a public reference
  artefact for the eventual `examples/fmeda-builder/README.md` stub.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| Classification ladder has unreachable "no mechanism" branch | medium | flattened ladder drafted; await human review |
| "SMvDU (Safe Fault Metric)" non-standard acronym in pitfall #5 | medium | flagged; needs source research before substitution |
| SPFM/LFM intro lines invert metric direction ("Coverage of *undetected*") | low | three-line rewrite drafted; await human review |
| `failure_mode_overrides.distribution_pct: 0.3` likely 100× off | low | flagged; needs maintainer call on unit convention |
| `#REF/#DIV/0` lacks `!` punctuation in Step 4 close | low (obs) | flagged; suite-wide style item |

**No code edits committed in this run.** Two medium-severity findings (logic bug
in Classification ladder + non-standard acronym) plus three low-severity items.
This is the **first** fmeda-builder polish pass — earlier passes targeted
description-quality and trigger-list shape, but this run uncovered actual math /
content issues. Recommend the Classification ladder fix gets prioritized in a
maintainer touch-up rather than carried as another polish-log appendix; the logic
bug is reachable and would produce a wrong rating for an unmechanized failure
mode if read literally.
