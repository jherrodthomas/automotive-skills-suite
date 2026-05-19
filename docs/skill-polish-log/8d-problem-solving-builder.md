# 8d-problem-solving-builder polish log

_Polish target for W21 (issue [#6](https://github.com/jherrodthomas/automotive-skills-suite/issues/6), carryover from W20). Reviewer: autonomous daily-standup task._

---

## 2026-05-19 — first POLISH pass (W21 Tuesday)

**Mode:** POLISH (Tuesday)
**File reviewed:** `skills/8d-problem-solving-builder.skill` (ZIP archive; SKILL.md is 2,122 bytes / 35 lines).
**DoD recap (from `docs/weekly/WEEK-2026-W21.md`):**
description names all 8 disciplines (D1–D8) and triggers include "warranty
response," "customer complaint," and "corrective action tracking."

### DoD verdict

| DoD check | Result | Evidence |
|---|---|---|
| All 8 disciplines (D1–D8) named in description | **PASS** | `Team (D1), Problem Description (D2), Interim Containment (D3), Root Cause Analysis (D4), Permanent Corrective Actions (D5-D6), Prevention (D7), and Team Recognition (D8)` — every label D1 through D8 appears |
| Trigger includes "warranty response" | **PASS** | verbatim in the "Use this skill whenever..." sentence (char ~430) |
| Trigger includes "customer complaint" | **PASS** | verbatim in the opening clause "for warranty, customer complaint, or field failure response" (char ~50, inside the 400-char fast-trigger window) |
| Trigger includes "corrective action tracking" | **PASS** | verbatim in the "Use this skill whenever..." sentence |

**Headline:** strict DoD passes cleanly — first POLISH this month with zero open
DoD items. Pattern from the W20 logs (one missing formal trigger phrase per skill,
either CAL allocation / CSR derivation / v3.1-v4.0 / safety goal) does **not**
repeat here. This was the right pick for "clear the carryover first" — the
backlog item turned out to be a smaller lift than the W20 skills.

### What's good

- **Description char count is generous.** Frontmatter description is 617 / 1024
  chars — substantial headroom for additive edits without hitting the cap.
- **Frontmatter is clean.** Both required keys present (`name`, `description`);
  YAML parses without complaint; no drift fields. Same shape as the W20 skills.
- **11-tab claim is exact and matches the generator.** Cross-checked SKILL.md's
  "11-tab xlsx" against `scripts/generate_8d.py`:
  - `00_Title_Page` · `01_Document_Control` · `02_D1_Establish_Team` ·
    `03_D2_Describe_Problem` · `04_D3_Interim_Containment` ·
    `05_D4_Root_Cause_Analysis` · `06_D5_Permanent_Corrective_Actions` ·
    `07_D6_Implement_Corrective_Actions` · `08_D7_Prevent_Recurrence` ·
    `09_D8_Recognize_Team` · `10_References`
  - Generator dict literally reports `"tabs": 11`. Count matches description.
- **Workflow section is tight.** 9 numbered steps (Capture → Team → Describe →
  Containment → RCA → Corrective → Implement/Verify → Prevent → Closure) map
  one-to-one onto the D1–D8 tab labels with the implement/verify split — same
  shape as the generator. No drift between doc, workflow narrative, and code.
- **"When to use this skill" list is concrete and well-scoped.** Six bullets
  (customer complaint, warranty claim, structured RCA, CAPA, audit closure,
  cross-functional action plan, management hand-off) cover the realistic
  invocation paths — none of them filler.
- **Description placement of trigger phrases is good.** All three DoD trigger
  phrases land inside the description (under the 1024-char cap), and "customer
  complaint" lands in the first 50 chars — the strongest possible trigger
  position. The pattern the W20 logs flagged ("formal trigger phrase outside
  the first 400 chars") does not occur here.

### What to fix

1. **`D5-D6` is hyphenated as a single bucket in the description, but the
   generator emits two separate tabs with distinct content.**
   The description says `Permanent Corrective Actions (D5-D6)` — implying one
   combined tab. The generator actually emits two: tab 6 is
   `D5_Permanent_Corrective_Actions` (design the fix), tab 7 is
   `D6_Implement_Corrective_Actions` (implementation tracker). The Workflow
   section in the SKILL.md body even separates them (step 6 "Permanent
   corrective actions" vs step 7 "Implement and verify"). The description is
   the only artefact that fuses them. Minor accuracy issue; does not affect
   triggering. Severity: **low**.

2. **The 11th tab (`10_References`) is absent from the description's tab
   enumeration.** The description names tabs for D1–D8 plus the two admin tabs
   (title, document control) — that totals 10. The actual workbook has an 11th
   References tab. The description's *count* ("11-tab xlsx") is correct but
   its *enumeration* implies only 10 named outputs. A reader counting from the
   enumeration will be off by one. Severity: **low**.

3. **No explicit mention of D0 (Plan/Prepare).** Many modern 8D references
   (Ford's TOPS-8D refresh, ASQ's 2020s guidance) name D0 explicitly as the
   prerequisite "is an 8D the right tool?" gate. This skill goes D1–D8 — which
   is still industry-standard and matches the AIAG materials — but a user
   trained on the D0–D8 variant who searches for "D0 prepare" won't match.
   Not a fix, just an observation; not in DoD. Severity: **low**, observation.

4. **Casual framings are thin compared to peer skills.** The trigger list
   covers formal terms well (8D problem solving, warranty response, complaint
   handling, corrective action tracking, cross-functional problem resolution)
   but no casual-phrasing safety nets like the ones `hara-builder` ships
   ("'I need the safety analysis for this ECU'"). Realistic casual invocations
   a user might type: "open an 8D for this defect," "build a CAPA for this
   complaint," "we got a warranty claim back from the OEM." None of these
   match verbatim. Severity: **low**, optional.

### Suggested edits (NOT applied this run)

The autonomous-edit allowlist for the daily-standup task is narrow — typo,
over-length description, missing required frontmatter field. None of the four
findings match that list: #1 and #2 are accuracy rewrites of the tab
enumeration, #3 is an observation (no action), and #4 is an editorial
trigger-coverage rewrite. **No edits committed today.** Captured here for the
next human review pass.

Minimal proposed rewrite of the description — splits D5/D6, adds References,
adds two casual phrasings, still under 1024 chars, all four DoD trigger phrases
preserved verbatim:

```
description: Generate an audit-ready 8D problem-solving workbook for warranty,
  customer complaint, or field failure response. Produces an 11-tab xlsx with
  title page and document control, plus one tab per discipline — Team (D1),
  Problem Description (D2), Interim Containment (D3), Root Cause Analysis (D4),
  Permanent Corrective Actions (D5), Implementation Tracker (D6), Systemic
  Prevention (D7), Team Recognition (D8) — and a References tab. Use this
  skill whenever the user mentions 8D problem solving, warranty response,
  customer complaint, complaint handling, corrective action tracking, CAPA, or
  cross-functional problem resolution — even casual phrasings like "open an
  8D for this defect" or "build a CAPA for this warranty claim." Always use
  this skill instead of producing freeform corrective action notes in chat.
```

Quick stats on the proposed rewrite:

| Check | Result |
|---|---|
| Char count | ~825 (under the 1024 cap) |
| All 8 disciplines named | yes (D1 through D8 each on its own clause) |
| `warranty response` first appears | trigger list (~char 555) |
| `customer complaint` first appears | char ~50 (inside 400) |
| `corrective action tracking` first appears | trigger list (~char 610) |
| References tab named | yes |
| Casual framings | two ("open an 8D for this defect", "build a CAPA for this warranty claim") |

### Other observations (not fixes, just notes for future passes)

- The SKILL.md body has **no "Files in this skill" tree** block (the safety
  builders do). Adding one would make the absence of a `references/` directory
  (compare `cs-concept-builder` and `aspice-assessment-builder`, both of which
  ship reference markdown bundled alongside SKILL.md) visible. This skill
  ships only `scripts/` and `examples/` — no `references/`. That's likely
  fine for 8D (it's a process method, not a standards-laden artefact), but a
  reader scanning for "what reference material does this skill carry?" gets
  no answer.
- The `recalc.py` and `scripts/office/soffice.py` helpers are present in the
  archive — same shared pair as every other builder in the suite. Consistent.
- The `examples/sample_8d_input.json` is **non-trivial** (~5 KB, populated
  with a realistic warranty scenario per the binary preview). Worth a human
  glance next time the skill comes up — examples this fleshed-out are the
  kind of thing a junior engineer can copy-modify, which is exactly the point.
- The workflow narrative groups D5+D6 into step 6 "Permanent corrective
  actions" but the tab list separates them. Minor doc-vs-code drift inside
  SKILL.md itself, not just the frontmatter. Could be unified to either
  pattern next pass — the generator (two tabs) is the source of truth, so the
  SKILL.md body should align to the two-tab split.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| D5/D6 hyphenated as one bucket in description | low | proposed rewrite drafted; await human review |
| References tab absent from enumeration | low | folded into the same rewrite |
| D0 not mentioned | low (obs) | flagged; not in DoD, no action |
| Casual framings thin | low (optional) | included in proposed rewrite |

**No code edits committed in this run.** Issue #6 stays open with this log
linked from the journal entry. The W20 pattern — one missing formal trigger
phrase per polish target — broke today: 8d-problem-solving-builder is the
first polish target this month where the strict DoD passes on the existing
description with no rewrite required. The findings here are accuracy and
casual-coverage polish rather than DoD gaps. Recommendation for tomorrow's
POLISH (W21 #7 dbc-builder): expect the W20 pattern to resume; comms cluster
hasn't been touched yet so trigger-coverage drift is plausible.
