# Polish log — ppap-package-builder

## 2026-07-07 (POLISH run, weekly-target #39)

**What's good**
- Generator runs clean end-to-end on the bundled sample (`examples/sample_ppap_input_esc.json`) and produces the promised 22-tab workbook covering all 18 PPAP elements plus PSW.
- SKILL.md frontmatter is valid (desc 555 chars, well under 1024); workflow doc, methodology, element-requirements, and submission-level references are thorough.
- Reviewer partnership is documented in `ppap-checklist-reviewer`'s SKILL.md ("natural partner" note).

**What was fixed this run (applied)**
- **[low] Sheet title over Excel's 31-char limit:** `05_Element3_Engineering_Approval` (32 chars) renamed to `05_Element3_Eng_Approval` in `scripts/generate_ppap_package.py` and the tab list in SKILL.md. Zero cross-references existed (verified via grep of reviewer scripts, README, examples), so the rename is safe. Smoke-tested: rebuilt package from the updated .skill zip, tab present, openpyxl warning count reduced from 2 to 1.

**What to fix (deferred — needs coordinated builder+reviewer change)**
- **[med] Second over-limit sheet title:** `09_Element7_Control_Plan_Reference` (34 chars) still exceeds 31. NOT renamed this run because `ppap-checklist-reviewer` exact-matches this name (`scripts/ppap_probe.py:256`, `scripts/check_definitions.py:313`) and its keyword fallback `["control plan"]` uses a space, which can never match an underscore-styled tab name. Suggested coordinated edit: rename to `09_Element7_Control_Plan_Ref` in builder + update both reviewer references (or make the fallback normalize `_`/space).
- **[med] Pre-existing Element-18 probe chain-break:** builder emits tab `20_Element18_Status`, but the reviewer probe expects `20_Element18_Submission_Status_Matrix` with fallback keyword `["submission status"]` — neither matches, so element submission statuses are silently never probed today. Same fix pattern as above (align names or normalize the keyword matcher).
- **[low] Reviewer stem mismatch (issue #39):** reviewer file is `ppap-checklist-reviewer.skill`, not `ppap-package-checklist-reviewer.skill`. STATUS regen re-honors the historical alias as of today, so the 🔴 flag is cleared. Durable options: (a) codify the alias in README as intentional, or (b) rename the reviewer .skill (outer file + inner dir + frontmatter `name:` + its SKILL.md cross-ref + examples/README rows) — a breaking rename for installed users, so it should be a deliberate human/PLAN decision, not an autonomous one.

**Severity:** med (the two deferred probe mismatches degrade the builder→reviewer chain; the applied fix was low-risk)
