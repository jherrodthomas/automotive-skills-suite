# aspice-assessment-builder polish log

_Polish target for W20 (issue [#5](https://github.com/jherrodthomas/automotive-skills-suite/issues/5)). Reviewer: autonomous daily-standup task._

---

## 2026-05-14 — first POLISH pass

**Mode:** POLISH (Thursday)
**File reviewed:** `skills/aspice-assessment-builder.skill` (ZIP archive; SKILL.md is 4,705 bytes / 95 lines).
**DoD recap (from `docs/weekly/WEEK-2026-W20.md`):**
confirm the 13-tab output list in the description matches the actual builder schema,
and the trigger list mentions both v3.1 and v4.0.

### DoD verdict

| DoD check | Result |
|---|---|
| 13-tab list matches builder schema | **PASS** |
| Trigger list mentions v3.1 and v4.0 | **PARTIAL** — versions are in the description's opening clause but not in the "Use this skill whenever..." trigger list |

### What's good

- **13-tab list is exact.** Cross-checked the SKILL.md "Output structure (13 tabs)"
  table (rows 00–12) against the `tabs` list in `scripts/generate_aspice_assessment.py`.
  All 13 entries match one-for-one, in order: Title Page, Document Control, Assessment
  Scope, Process Reference Model, Process Assessment Model, BP Assessment, GP Assessment,
  PA Rating, Capability Level, Capability Profile, Strengths/Weaknesses, Assessor
  Sign-off, References. The frontmatter description's "Produces 13 tabs including ..."
  partial enumeration names only real tabs and the count is correct. This is the
  primary DoD check and it passes cleanly — no drift between doc and schema.
- **Frontmatter is clean.** Both required keys present (`name`, `description`); no drift
  fields; YAML parses without complaint. Description is ~530 / 1024 chars — very
  comfortable headroom for a polish-only rewrite.
- **The ASPICE rating algorithm section is accurate.** The CL0–CL5 determination ladder
  in SKILL.md matches the `pa_mapping` in the generator (PA1.1, PA2.1–2.2, PA3.1–3.2,
  PA4.1–4.2, PA5.1–5.2) and the standard ASPICE PAM aggregation rule (each level needs
  the level below achieved AND both of its PAs ≥ Largely). Mentor-quality content, not
  filler — a junior assessor could rate a process correctly from this section alone.
- **N/P/L/F indicator scale is introduced once, early, and reused consistently.** Defined
  in the intro paragraph and echoed in Step 3 and the tab table. No ambiguity.
- **Workflow is correctly ordered** — scope capture → enumerate processes/practices →
  rate indicators → generate → review. The Step 5 review checklist (BP/GP evidence
  completeness, indicator ratings, PA aggregation, achieved CL) maps to the exact tabs
  a human would sanity-check.

### What to fix

1. **DoD partial miss: v3.1 / v4.0 are not in the trigger list.**
   The versions appear in the description's first clause ("ASPICE PAM v3.1/v4.0 process
   assessment workbook") but the "Use this skill whenever the user needs to ..." trigger
   list does not name them. A user invoking with "run an ASPICE v4.0 assessment" still
   matches on "ASPICE ... process assessment", so trigger reliability is not broken — but
   per the strict W20 DoD this is the one open item. Severity: **low**.

2. **No workflow step instructs reading the bundled references.** The archive ships
   `references/aspice_pam_3_1.md` and `references/methodology.md`, but unlike
   `item-definition-builder` (which has an explicit "Step 2 — Read the references"),
   this SKILL.md never tells Claude to read them before generating. The reference
   content is therefore dead weight at runtime. Severity: **low**.

3. **References directory has no v4.0 companion file.** The skill advertises v3.1/v4.0
   support and the References *tab* lists "ASPICE PAM v4.0" / "ASPICE PRM v4.0", but
   `references/` only contains `aspice_pam_3_1.md`. A v4.0 reference (or a note that the
   v3.1 file covers both with deltas called out) would close the gap. Severity: **low**,
   observation.

4. **`.placeholder` cruft file in the archive.** `unzip -l` shows an empty
   `aspice-assessment-builder/.placeholder` — almost certainly a leftover empty-dir
   marker from packaging. Harmless but untidy; worth dropping next time the `.skill` is
   repackaged. Severity: **low**, observation.

### Suggested edits (NOT applied this run)

The autonomous-edit allowlist for the daily-standup task is narrow — typo / over-length
description / missing-required-frontmatter-field. None of the four findings match that
list: #1 is an editorial trigger-coverage rewrite, #2 is a workflow-content addition,
and #3/#4 are archive-content changes. **No edits committed today.** Captured here for
the next human review pass.

Minimal proposed rewrite of the description — folds v3.1/v4.0 into the trigger list and
adds the CL range, still well under 1024 chars, 13-tab claim untouched:

```
description: Generate a multi-tab ASPICE PAM v3.1/v4.0 process assessment workbook
  with capability level ratings, base practices, generic practices, and process
  attribute assessments. Produces 13 tabs including Title Page, Document Control,
  Assessment Scope, Process Reference Model, Process Assessment Model, BP/GP
  assessments, Capability Level determinations, strengths/weaknesses analysis, and
  assessor sign-off blocks. Use this skill whenever the user needs to conduct or
  document an ASPICE v3.1 or v4.0 process assessment, evaluate process maturity,
  rate capability levels (CL0-CL5), or create audit-ready assessment evidence.
```

For finding #2, the minimal fix is a one-line addition to Step 2 (or a new half-step):
"Before generating, read `references/methodology.md` and `references/aspice_pam_3_1.md`
for the PA aggregation rules and indicator-rating guidance." — left for human review
since it is workflow content, not a surgical fix.

### Other observations (not fixes, just notes for future passes)

- SKILL.md uses ASCII hyphens for step headers (`### Step 1 - Capture ...`) where
  `hara-builder` and `cs-concept-builder` use em-dashes (`### Step 1 — ...`). Purely
  cosmetic; not worth an autonomous edit, but a suite-wide style pass could normalize it.
- The SKILL.md has **no "Files in this skill" tree** block (item-definition-builder and
  the safety builders do). Optional, but adding one would make the `.placeholder` cruft
  and the missing v4.0 reference visible to anyone reading the skill. Docs item, not a
  polish item.
- The generator is a single clean script (`generate_aspice_assessment.py`) plus the
  shared `recalc.py` / `office/soffice.py` helpers — consistent with the rest of the
  suite. The `tabs` list at the bottom of the script is the source of truth for the
  13-tab claim and is easy to diff against SKILL.md on future passes.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| v3.1/v4.0 absent from trigger list | low | proposed rewrite drafted; await human review |
| No step instructs reading `references/` | low | one-line Step 2 addition proposed; await human review |
| No v4.0 reference file in archive | low (obs) | flagged for human / repackage pass |
| `.placeholder` cruft in archive | low (obs) | flagged for human / repackage pass |

**No code edits committed in this run.** Issue #5 stays open with this log linked from
the journal entry. This is the **third consecutive POLISH finding** with the same core
shape — fundamentals are sound (13-tab list exact, rating algorithm accurate, frontmatter
clean) but a formal trigger phrase (here: the version numbers) lives outside the active
trigger list. `hara-builder` (#3, "safety goal"), `cs-concept-builder` (#4, "CSR
derivation"/"CAL allocation"), and now `aspice-assessment-builder` (#5, "v3.1/v4.0") all
shipped this pattern. The W21 PLAN should treat a suite-wide trigger-coverage DoD audit
as a first-class target rather than continuing to catch one per POLISH day.
## 2026-06-25 (autonomous POLISH, issue #28)

**What's good**
- Frontmatter is well-formed (name + description), description is a healthy length and trigger-rich.
- Clear 5-step workflow, 13-tab output table, and an N/P/L/F indicator scheme that matches ASPICE practice.
- Generator leaves Achieved CL blank for the assessor rather than guessing — appropriate, since CL is a judgement call.

**What to fix**
- The documented CL determination algorithm understated the ISO/IEC 33020 rule. It said each level only requires its own PAs to be ">= L", omitting that all *lower* PAs must be **Fully** achieved (F). As written, a process with PA1.1=L and PA2.1/PA2.2=L would read as CL2, which is incorrect — PA1.1 must be F for CL2.

**Applied this run**
- Rewrote the "ASPICE rating algorithm (CL determination)" section to state the correct ISO/IEC 33020 rule: attributes *at* the target level L-or-F, every attribute *below* it Fully achieved (F). Added an explicit note that a Largely-achieved lower attribute does not satisfy a higher-level claim. Doc-only change; the generator script is untouched (it does not auto-compute CL, so no code mismatch is introduced).

**Severity:** medium (correctness of stated rating rule; could mislead an assessor reading the skill as authority).

**Follow-ups / not done this run**
- Consider having generate_aspice_assessment.py optionally auto-compute Achieved CL from PA ratings using this rule (would need a PA-rating input block). Larger change — deferred.
- recalc.py contains no CL logic; if auto-compute is added, that's where it belongs.
