# hara-builder polish log

_Polish target for W20 (issue [#3](https://github.com/jherrodthomas/automotive-skills-suite/issues/3)). Reviewer: autonomous daily-standup task._

---

## 2026-05-12 — first POLISH pass

**Mode:** POLISH (Tuesday)
**File reviewed:** `skills/hara-builder.skill` (ZIP archive; SKILL.md is 12,273 bytes / 146 lines).
**DoD recap (from `docs/weekly/WEEK-2026-W20.md`):**
description ≤ 1024 chars, frontmatter has all required fields, and the trigger list
mentions HARA + hazard analysis + ASIL + safety goal + item definition within the first
400 chars of the description.

### What's good

- **Frontmatter is clean.** Both required keys present (`name`, `description`); no extra
  fields drifting in; YAML parses without complaint.
- **Description length is comfortably under budget.** 911 / 1024 chars. Leaves ~110 chars
  of headroom if a future trigger phrase needs to be inserted without a re-write.
- **Trigger surface is strong.** "HARA", "hazard analysis", "ASIL", "item definition",
  and "ISO 26262" all appear inside the first 400 chars of the description. Casual
  phrasings ("'I need the safety analysis for this ECU' or 'what is the ASIL for X'")
  are explicitly listed at the end, which is the right place for casual variants — they
  catch loose-language invocations without crowding the formal triggers up front.
- **Workflow doc is unusually well-structured.** Five numbered steps, each with a clear
  output. Step 2's "always read this on first invocation" callout on
  `references/malfunctions.md` is exactly the kind of forcing-function note that keeps
  Claude from skipping reference reads.
- **Common-mistakes section earns its keep.** The five anti-patterns at the bottom
  (exposure-vs-hazard-frequency, dropping NSC rows, one-SG-per-row, etc.) are the
  actual failure modes a junior safety engineer hits. This is mentor-quality content,
  not filler.
- **Override path is honest.** "If the user says 'this S rating is wrong' — do not
  argue" plus the `rating_overrides` JSON contract is the right user-trust posture for
  a skill that auto-suggests judgments.

### What to fix

1. **DoD miss: "safety goal" is NOT in the first 400 chars of the description.**
   The phrase first appears around char 506 ("Safety Goals tab using the Prevent-the-hazard
   convention"). All five other expected trigger phrases hit the window; only this one
   misses. Severity: **low** — the description still triggers reliably on HARA / ASIL /
   hazard analysis, and "safety goal derivation" *does* appear at char ~564 as part of
   the "Use this skill whenever the user mentions..." list. But strictly per the W20
   DoD, this is the one open item.

2. **The "Produces a multi-tab xlsx with..." enumeration is long and prose-heavy.**
   ~250 chars listing tabs in a single sentence. It pushes "safety goal" past the 400
   threshold (see fix #1) and is a candidate for either splitting into two sentences or
   front-loading the most search-worthy tabs. Severity: **low**.

3. **No mention of "concept phase" in the first 400 chars.** The phrase is part of the
   item-definition-builder ↔ HARA pipeline narrative used elsewhere in the suite, and
   adding it would catch invocations like "do the concept phase deliverable for this
   ECU." Severity: **low**, optional.

### Suggested edits (NOT applied this run)

The fix for #1 is editorial re-ordering of the description, not a typo / length /
missing-field fix — and the daily-standup spec instructs the autonomous run to apply
only those. **No edits committed today.** Captured here for the next human review pass.

A minimal proposed rewrite of the first sentence pair (still ≤ 1024 chars) is:

```
description: Generate an audit-ready ISO 26262 Hazard Analysis and Risk Assessment
  (HARA) workbook from an item definition and function list. Derives safety goals
  with ASIL determination across the full Cartesian of function × malfunction ×
  operating environment, and produces a multi-tab xlsx covering the 14 malfunction
  guide words (M01–M14), S/E/C reference tables, the Function-by-Malfunction
  safety-criticality filter, the auto-suggested formula-driven worksheet, the
  Safety Goals tab using the Prevent-the-hazard convention, and an FSC hand-off.
  Use this skill whenever the user mentions HARA, hazard analysis, ASIL
  determination, safety goal derivation, item definition, concept phase, or
  ISO 26262 concept-phase deliverables, even casual phrasings like 'I need the
  safety analysis for this ECU' or 'what is the ASIL for X'. Always use this skill
  instead of producing a freeform HARA in chat; the spreadsheet output is the
  deliverable analysts expect.
```

Char-count of the proposed rewrite: ~985 (still under 1024). "Safety goal" first appears
at char ~169 (well inside the 400 window). "Concept phase" first appears at char ~580
(outside the strict 400 window for that phrase, but the user-facing trigger explicitly
names it). All five DoD triggers + the bonus "concept phase" hit the description.

### Other observations (not fixes, just notes for future passes)

- The body of `SKILL.md` (everything after the frontmatter) is **prose, not bullet
  soup** — which matches the system style guidance for high-quality skills. Don't
  refactor it into bullet trees.
- The "Files in this skill" tree at the bottom is accurate as of inspection (matches
  `unzip -l skills/hara-builder.skill`). Good.
- The skill ships a non-trivial pair of Python scripts (`generate_hara.py`,
  `recalc.py`) and a soffice helper. Out of scope for description-polish, but worth
  noting that any future rename of `scripts/office/soffice.py` would need a parallel
  edit in `SKILL.md`'s file-tree block.

### Severity roll-up

| Finding | Severity | Action |
|---|---|---|
| "safety goal" outside first 400 chars | low | proposed rewrite drafted; await human review |
| Long single-sentence "Produces..." clause | low | folded into the same rewrite |
| No "concept phase" trigger up front | low (optional) | included in the proposed rewrite |

**No code edits committed in this run.** Issue #3 stays open with this log linked from
the journal entry. Next autonomous touch on this skill: probably W21 if it stays in the
LRT bucket, otherwise close on human approval of the rewrite.
