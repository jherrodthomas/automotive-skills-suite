# cs-concept-builder polish log

_Polish target for W20 (issue [#4](https://github.com/jherrodthomas/automotive-skills-suite/issues/4)). Reviewer: autonomous daily-standup task._

---

## 2026-05-13 — first POLISH pass

**Mode:** POLISH (Wednesday)
**File reviewed:** `skills/cs-concept-builder.skill` (ZIP archive; SKILL.md is 9,760 bytes / ~120 lines).
**DoD recap (from `docs/weekly/WEEK-2026-W20.md`):**
description reviewed for accuracy of the six security properties (AAICAN), and
trigger phrases include both formal terms (CSR derivation, CAL allocation) and
casual framings ("derive cyber controls from goals").

### What's good

- **AAICAN list is correct, complete, and in canonical order.** The six properties
  appear as `(Authentication, Authorization, Integrity, Confidentiality, Availability,
  Non-repudiation)` — all six letters of AAICAN, no duplicates, no omissions — and
  all sit comfortably inside the first 400 chars of the description (the parenthetical
  ends at char ~358). This is the DoD's "accuracy" check; passes cleanly.
- **Frontmatter is clean.** Both required keys present (`name`, `description`); no
  drift fields; YAML parses without complaint. Description is 784 / 1024 chars with
  ~240 chars of headroom — comfortable runway for a polish-only re-write without
  hitting the cap.
- **Pipeline framing is explicit and correct.** The description names both upstream
  (`reading a CS Goals xlsx (produced by cs-goals-builder)`) and downstream (`produces
  the CSI (Cybersecurity Implementation) hand-off`) artefacts, which is exactly the
  cross-builder linkage that prevents users from invoking this skill before the TARA →
  CS Goals step has run. Same pattern as `fsc-builder`'s HARA→FSC linkage.
- **Workflow doc is strong.** Step 2's enumeration of node types (gateways → ECUs →
  memory → key stores → channels → debug-ifs) is the right order — network-facing
  first, then progressively inward — and the closing sentinel question ("Is there
  any node that, if compromised or disabled, could violate a cybersecurity goal but
  isn't on this list?") is the kind of forcing-function that catches the 10% of nodes
  analysts skip on first pass. Mentor-quality.
- **Common-pitfalls section earns its keep.** All five anti-patterns (skipped debug
  interfaces, missing key-management CSRs, authn-vs-authz conflation, over-specifying
  algorithms, missing non-repudiation for audit-critical actions) are real failure
  modes a junior security engineer hits. None are filler.
- **Six-property derivation prose is concrete.** Each property comes with the
  attacker-question framing ("Does the attacker impersonate or forge identity?" etc.).
  That's what makes the property selection mechanical rather than judgment-only.

### What to fix

1. **DoD miss: "CAL allocation" is NOT in the description at all.**
   The closest phrasing is `allocates CAL per architectural element` (char ~430),
   which uses the verb form but not the noun phrase the DoD asks for. Severity: **low**
   — the description still triggers reliably on the surrounding terms (CSR, six
   security properties, Cybersecurity Concept), but per the strict W20 DoD this is the
   one missing formal trigger.

2. **DoD miss: "CSR derivation" is past the 400-char threshold.**
   The phrase first appears at char ~607 inside the "Use this skill whenever the user
   mentions Cybersecurity Concept, CSR derivation, threat modeling requirements, or
   deriving cybersecurity controls from goals" trigger list. Falls inside the
   description (under the 1024-char cap), but outside the 400-char fast-trigger
   window. Same shape as yesterday's `hara-builder` "safety goal" miss. Severity:
   **low**.

3. **DoD partial: casual framings are thin.**
   Only one casual variant is present (`deriving cybersecurity controls from goals`).
   The DoD's example (`derive cyber controls from goals`) is essentially covered but
   not verbatim, and there are no alternate-phrasing safety nets like the ones
   `hara-builder` ships ("'I need the safety analysis for this ECU' or 'what is the
   ASIL for X'"). Severity: **low**, optional.

4. **`CSI handoff` trigger only appears with the long form.**
   The description writes `CSI (Cybersecurity Implementation) hand-off` (with paren
   gloss and hyphen). A user typing "do the CSI handoff for this ECU" (no parens, no
   hyphen) will still hit `CSI`, but a pattern-matching trigger pass on the literal
   phrase `CSI handoff` would miss. Severity: **low**, optional.

### Suggested edits (NOT applied this run)

The autonomous-edit allowlist for the daily-standup task is narrow — typo / over-length
/ missing-required-frontmatter-field. None of the four findings match that list:
all are trigger-coverage edits, which are editorial rewrites, not surgical fixes.
**No edits committed today.** Captured here for the next human review pass.

A minimal proposed re-write of the description (still inside 1024 chars, AAICAN
preserved verbatim, all four DoD trigger phrases now in the first 400 chars):

```
description: Generate an audit-ready ISO/SAE 21434 Cybersecurity Concept workbook
  with CSR derivation, CAL allocation, and CSI handoff by reading a CS Goals xlsx
  (produced by cs-goals-builder) plus a system block-diagram JSON. Builds one
  requirement-derivation tree per cybersecurity goal across six security
  properties (Authentication, Authorization, Integrity, Confidentiality,
  Availability, Non-repudiation), generates one CSR per goal-property-node
  combination, allocates CAL per architectural element, proposes verification
  methods per CAL, and produces the CSI (Cybersecurity Implementation) hand-off.
  Use this skill whenever the user mentions Cybersecurity Concept, CSR derivation,
  CAL allocation, threat-modeling requirements, or deriving cybersecurity controls
  from goals — even casual framings like "derive cyber controls from goals" or
  "what CSRs do I need for this ECU". Always use this skill instead of producing
  a freeform Cybersecurity Concept in chat.
```

Quick stats on the proposed rewrite:

| Check | Result |
|---|---|
| Char count | ~970 (under the 1024 cap) |
| `CSR derivation` first appears | char ~85 (inside 400) |
| `CAL allocation` first appears | char ~99 (inside 400) |
| `CSI handoff` first appears | char ~117 (inside 400) |
| AAICAN list intact | yes, char ~300–415 |
| Casual framings | two (`derive cyber controls from goals`, `what CSRs do I need for this ECU`) |

### Other observations (not fixes, just notes for future passes)

- The SKILL.md body is **prose with one well-bounded reference-table block** for the
  10 output tabs — matches system style guidance. Don't refactor it.
- The "Files in this skill" tree at the bottom is accurate against `unzip -l
  skills/cs-concept-builder.skill`: SKILL.md, scripts/{generate_cs_concept.py,
  cs_goals_reader.py, recalc.py, office/{__init__.py, soffice.py}}, references/
  {methodology.md, csr_templates.md}, examples/sample_cs_concept_input_esc.json. OK.
- The skill ships a non-trivial pair of Python scripts plus a soffice helper, same
  as the safety-side builders. Out of scope for description-polish, but worth noting
  that the CAL inheritance logic in `generate_cs_concept.py` is the one piece a
  human reviewer should re-validate against ISO/SAE 21434 §15 next time the file
  comes up — the SKILL.md asserts "If a node is allocated multiple CSRs from
  different CSGs, its CAL is the maximum" and that should match what the script does.
- The CSI-handoff tab is described as "placeholder cybersecurity implementation
  requirement rows" — that wording is fine, but a future docs pass should make
  clear that the CSI is its own SDLC artefact (not produced by this skill), so the
  rows here are intentionally skeleton-only. Not a polish item — a docs item.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| `CAL allocation` absent from description | low | proposed rewrite drafted; await human review |
| `CSR derivation` outside first 400 chars | low | folded into the same rewrite |
| Casual framings thin / not verbatim | low (optional) | included in the proposed rewrite |
| `CSI handoff` plain form absent | low (optional) | included in the proposed rewrite |

**No code edits committed in this run.** Issue #4 stays open with this log linked
from the journal entry. Next autonomous touch on this skill: probably W21 if it
stays in the LRT bucket, otherwise close on human approval of the rewrite. The
pattern (description hits AAICAN cleanly but misses one or two formal trigger
phrases in the first 400 chars) is now the second consecutive POLISH finding —
`hara-builder` (W20-T1, Tue) and `cs-concept-builder` (W20-T2, today) both shipped
this same shape. Worth flagging in the next PLAN as a suite-wide DoD audit
candidate rather than per-skill chasing.

---

## 2026-06-02 — W23 POLISH pass (four-week carryover from W20)

**Mode:** POLISH (Tuesday)
**File reviewed:** `skills/cs-concept-builder.skill` (ZIP archive; inner SKILL.md is 9,760 bytes / 133 lines).
**Tracking issue:** [#4](https://github.com/jherrodthomas/automotive-skills-suite/issues/4) (open, four-week carryover).
**Plan reference:** target #1 in `docs/weekly/WEEK-2026-W23.md` (top of W23 priority list).

### File-state check vs. previous passes

The archive is unchanged since the W20 inspection — every inner-file mtime is
`2026-05-01 07:55–07:58`, the SKILL.md is still 9,760 bytes / 133 lines, and the
top-level `.skill` was last touched by the initial seed commit f8d97993 on
2026-05-01. No drift, no regressions, nothing to undo.

Re-ran the W20 trigger-coverage check fresh against the current bytes rather
than trusting the prior log:

| DoD check (W20) | This pass |
|---|---|
| Description ≤ 1024 chars | **PASS** — 784 chars (unchanged, ~240 headroom) |
| Frontmatter has required fields (`name`, `description`) | **PASS** |
| AAICAN list intact in canonical order | **PASS** — all six properties present at offsets 255 / 271 / 286 / 297 / 314 / 328 (all inside the 400-char window) |
| `Cybersecurity Concept` in first 400 chars | **PASS** — offset 38 |
| `ISO/SAE 21434` in first 400 chars | **PASS** — offset 24 |
| `six security properties` in first 400 chars | **PASS** — offset 230 |
| `CSR` in first 400 chars | **PASS** — offset 360 (verb form, not the noun phrase `CSR derivation`) |
| `CAL` standalone | **MARGINAL** — offset 410, just outside the 400-char window |
| `CSR derivation` (formal trigger) in first 400 chars | **FAIL** — offset 607 (same gap W20 found; the noun phrase only appears inside the "Use this skill whenever the user mentions…" trigger list) |
| `CAL allocation` (formal trigger) anywhere in description | **FAIL** — literal noun phrase absent entirely. Closest is the verb form `allocates CAL per architectural element` at offset 400. |
| `CSI handoff` (plain form) in description | **FAIL** — only the parenthesized form `CSI (Cybersecurity Implementation) hand-off` appears, at offset 497. |
| Typo scan (`teh recieve seperate occured definately accomodate begining wether adress arguement existance reccommend reccomend`) | **PASS** — no hits. |

Archive file-tree cross-check against `unzip -l` matches the "Files in this
skill" block in the SKILL.md footer exactly — same nine entries
(`SKILL.md`, `scripts/{generate_cs_concept.py, cs_goals_reader.py, recalc.py,
office/{__init__.py, soffice.py}}`, `references/{methodology.md,
csr_templates.md}`, `examples/sample_cs_concept_input_esc.json`). No
dbc-builder-style packaging drift here.

### Autonomous-edit allowlist decision

The standup spec defines the autonomous-edit allowlist narrowly: **typo,
description > 1024 chars, missing required frontmatter field**. None of the
three triggers fire here:

- Description is 784 chars — well under cap.
- Both required frontmatter keys (`name`, `description`) are present.
- Zero typos in the SKILL.md body or frontmatter.

The three open DoD findings are all editorial trigger-coverage rewrites of the
description, identical to the gaps caught in the W20 entry. Applying the
W20-proposed rewrite (~970 chars, reshuffles `CSR derivation` / `CAL
allocation` / `CSI handoff` into the first 400 chars and adds two casual
framings) would touch roughly **600 of the 784 description chars** — the same
"editorial restructure, not surgical fix" judgement the W22 hara-builder pass
applied to its proposed rewrite, and outside the standup's "small and shipped
beats big and broken" rule.

**No edits committed this run.** Findings carry forward unchanged.

### Why this is the fourth straight pass without a code edit

W20 (2026-05-13) drafted the rewrite. W21 PLAN deprioritized cs-concept in
favour of newer targets; no POLISH touch. W22 PLAN re-included cs-concept as
target #4 but the actual W22 POLISH cycle (uds → dfmea → hara) consumed all
three slots before reaching it — flagged in the W22 RELEASE journal as the
explicit reason cs-concept must lead W23. W23 PLAN promoted it to target #1,
which is why today's pass exists.

The pattern is now clear and worth stating out loud: **the cs-concept-builder
description is functionally fine** (84 chars over budget would change that;
missing frontmatter would change that; typos would change that) **but it
fails one specific DoD that the autonomous loop is not permitted to fix**.
Either the human approves the W20 rewrite and this issue closes, or the DoD
gets relaxed in a future PLAN, or the issue stays in carryover indefinitely.
A fifth POLISH pass next month would produce the same log entry for the same
reasons — diminishing returns are real here.

### Severity roll-up (unchanged from W20)

| Finding | Severity | Action |
|---|---|---|
| `CAL allocation` absent from description | low | W20-proposed rewrite drafted; await human review |
| `CSR derivation` outside first 400 chars | low | folded into the same proposed rewrite |
| `CSI handoff` plain form absent | low (optional) | folded into the same proposed rewrite |
| Casual framings thin / not verbatim | low (optional) | folded into the same proposed rewrite |

### Follow-ups for the human

1. **Decide on the W20-proposed rewrite of the description** (block quote in
   the 2026-05-13 entry above). If approved, it lands as a one-shot edit and
   issue [#4](https://github.com/jherrodthomas/automotive-skills-suite/issues/4)
   closes the same day. If rejected, the issue should be closed as `won't fix
   (outside autonomous scope)` to break the four-week loop.
2. **Suite-wide DoD audit** for the trigger-coverage gap is still recommended
   (`hara-builder`, `cs-concept-builder`, and likely `tara-builder` /
   `fmeda-builder` / `aspice-assessment-builder` carry the same shape). One
   focused audit replaces five carryover loops. Captured in the W23 PLAN
   notes; still not scheduled.
3. **Optional:** fold the autonomous-edit allowlist into the standup spec
   explicitly so future runs have a citeable rule for declining editorial
   rewrites without re-deriving the decision each visit.

---

## 2026-07-21 — W30 POLISH pass (issue #44: NUL-strip — FIXED)

**Mode:** POLISH (Tuesday)
**File reviewed:** `skills/cs-concept-builder.skill`
**Tracking issue:** [#44](https://github.com/jherrodthomas/automotive-skills-suite/issues/44) (W30 target: strip 3,361 trailing NUL bytes from the generator). Related: [#43](https://github.com/jherrodthomas/automotive-skills-suite/issues/43) (chain repair — NOT attempted this pass, see below).

### What was found

- `scripts/generate_cs_concept.py` was 19,114 bytes with **exactly 3,361 NUL bytes, all trailing** (content ends cleanly at `if __name__ == "__main__":\n    main()\n`; zero interior NULs).
- The NULs were not cosmetic: `python3 -m py_compile` **failed** with `ValueError: source code string cannot contain null bytes`. The generator was un-runnable as shipped — anyone invoking the skill got a hard error at Step 3.

### Fix applied (small, mechanical, verified)

1. Extracted archive; stripped trailing NULs only (`data.rstrip(b'\x00')`) → 15,753 bytes.
2. `py_compile` passes post-strip; the other three scripts (`cs_goals_reader.py`, `recalc.py`, `office/soffice.py`) also verified clean.
3. Repacked with `zip -X -D` — same 9 entries, no directory-entry drift, byte-identical content for the 8 untouched files.
4. Re-extracted the committed archive and re-verified compile + zero NULs.

Judgement call: this sits inside the "small obvious fix" allowlist (single-file mechanical corruption repair, no logic touched) and is the explicit DoD of #44, so it was applied rather than merely logged.

### NOT done: #43 reader rewrite

The cs-concept → cs-architecture chain repair is a logic rewrite of `cs_goals_reader.py` / downstream reader expectations — an editorial/structural change, not a surgical fix. Left for a dedicated pass per "small and shipped beats big and broken." Findings from the 2026-07-16 pass stand.

### NEW systemic finding: 13 more archives carry the same trailing-NUL corruption

Suite-wide scan (all 152 archives, all `.py/.md/.json` members) found the identical defect class — in every case 100% of NULs are trailing:

| Archive | Member | Trailing NULs |
|---|---|---|
| autosar-composition-checklist-reviewer.skill | scripts/dashboard.py | 14,048 |
| autosar-swc-checklist-reviewer.skill | scripts/dashboard.py | 14,064 |
| fsc-builder.skill | SKILL.md | 235 |
| fsc-checklist-reviewer.skill | SKILL.md | 167 |
| hara-builder.skill | SKILL.md | 266 |
| hara-builder.skill | scripts/generate_hara.py | 24 |
| hara-checklist-reviewer.skill | SKILL.md | 229 |
| hara-checklist-reviewer.skill | scripts/generate_checklist.py | 3,398 |
| pfmea-builder.skill | scripts/generate_pfmea.py | 617 |
| ppap-package-builder.skill | scripts/generate_ppap_package.py | 112 |
| safety-program-risk-register-checklist-reviewer.skill | scripts/build_dashboard.py | 36 |
| safety-program-risk-register-checklist-reviewer.skill | scripts/generate_checklist.py | 9,792 |
| secure-coding-guidelines-checklist-reviewer.skill | scripts/generate_checklist.py | 305 |
| sw-fmea-builder.skill | scripts/generate_sw_fmea.py | 97 |
| tsc-builder.skill | SKILL.md | 363 |
| tsc-checklist-reviewer.skill | SKILL.md | 165 |

Every `.py` member above fails `py_compile` today — i.e., **8 more skills ship broken generators/dashboards** (hara-builder among them, a flagship). The `.md` cases are lower-severity (renderers tolerate trailing NULs) but same root cause — likely the May-01/02 bulk-import tooling padded files to block boundaries. Severity: **high** for the .py cases.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| cs-concept generator trailing NULs | high | **FIXED this run** (issue #44 DoD met) |
| 13 more archives with trailing-NUL members (8 with broken .py) | high | logged; scan script reproducible; recommend Wed/Thu POLISH batch-fix under #46's audit umbrella or a dedicated issue at next PLAN |
| #43 chain break | med | untouched, carries |
| W20 description-rewrite proposal | low | still awaiting human decision |

## 2026-07-22 — Batch NUL-strip resolution (follow-through on 2026-07-21 findings)

Executed the Wed POLISH follow-up: batch-applied the verified strip+repack+py_compile
procedure to all 13 archives flagged in yesterday's suite-wide scan.

**What's good:** the fix generalized perfectly — every `.py` member that failed
`py_compile` before the strip compiles cleanly after (9 members across 8 archives,
including hara-builder's flagship generator and the two 14 KB autosar dashboard cases).
All `.md` members retain intact frontmatter. Suite-wide `.py`/`.md` NUL rescan: **CLEAN**.

**Correction to yesterday's findings:** the three `examples/*.xlsx` entries
(fsc-checklist-reviewer, hara-checklist-reviewer ×2, tsc-checklist-reviewer) were
**false positives**. Their "4 trailing NULs" are the zip EOCD record's own zero bytes
(comment_len=0 + offset high bytes) — legitimate structure, EOCD flush with EOF, zero
trailing garbage. Stripping them corrupts the file. They were left untouched; the count
of genuinely corrupted archives is 13, corrupted *text/script* members 19.

**Scan methodology fix:** any future NUL scan must exclude (or EOCD-verify) formats
that legitimately end in NUL bytes (zip-based: .xlsx, .docx, .pptx). Naive
`rstrip(b'\x00')` comparison over-flags them.

**Fixed members (all verified post-repack):**

| Archive | Member | NULs removed | Verification |
|---|---|---|---|
| autosar-composition-checklist-reviewer.skill | scripts/dashboard.py | 14,048 | py_compile FAIL→OK |
| autosar-swc-checklist-reviewer.skill | scripts/dashboard.py | 14,064 | py_compile FAIL→OK |
| fsc-builder.skill | SKILL.md | 235 | frontmatter intact |
| fsc-checklist-reviewer.skill | SKILL.md | 167 | frontmatter intact |
| hara-builder.skill | SKILL.md | 266 | frontmatter intact |
| hara-builder.skill | scripts/generate_hara.py | 24 | py_compile FAIL→OK |
| hara-checklist-reviewer.skill | SKILL.md | 229 | frontmatter intact |
| hara-checklist-reviewer.skill | scripts/generate_checklist.py | 3,398 | py_compile FAIL→OK |
| pfmea-builder.skill | scripts/generate_pfmea.py | 617 | py_compile FAIL→OK |
| ppap-package-builder.skill | scripts/generate_ppap_package.py | 112 | py_compile FAIL→OK |
| safety-program-risk-register-checklist-reviewer.skill | scripts/build_dashboard.py | 36 | py_compile FAIL→OK |
| safety-program-risk-register-checklist-reviewer.skill | scripts/generate_checklist.py | 9,792 | py_compile FAIL→OK |
| secure-coding-guidelines-checklist-reviewer.skill | scripts/generate_checklist.py | 305 | py_compile FAIL→OK |
| sw-fmea-builder.skill | scripts/generate_sw_fmea.py | 97 | py_compile FAIL→OK |
| tsc-builder.skill | SKILL.md | 363 | frontmatter intact |
| tsc-checklist-reviewer.skill | SKILL.md | 165 | frontmatter intact |

Severity: was **high** (8 skills shipped un-compilable generators/dashboards) → **resolved**.
The May-01/02 bulk-import padding fault family is now fully remediated across the suite;
#46's audit can drop the NUL question and focus on its original chain-contract scope.
