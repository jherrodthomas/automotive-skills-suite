# Polish log — cs-architecture-builder

## 2026-07-16 (POLISH, Thu)

**Selection reason:** No open issues (tracker empty), no true orphans (item-def/ppap paired via docs/PAIRING_ALIASES.md). Least-recently-touched tier (2026-05-01, 76 days); chose cs-architecture-builder as it had no polish log yet (cs-goals/cs-architecture were the only unpolished builders in that tier).

**What's good:**
- Frontmatter valid: `name` + `description` (757/1024 chars), clear trigger phrases.
- All 6 Python scripts compile clean (py_compile).
- Smoke test PASSED: generator run against the bundled `examples/sample_cs_arch_input_esc.json` plus a synthetic CS Concept xlsx (built to the reader's contract) → 12-tab workbook, 7 CS elements, 14 CSR allocations, all sheet titles ≤31 chars.
- Forward chain intact: `cs-architecture-checklist-reviewer` probes exactly the tab names this builder emits (verified by grep of reviewer scripts — 10/10 sheet-name matches).

**What to fix:**
- **[high] Upstream chain-break: cs-concept-builder output does not match this skill's reader.** `scripts/cs_concept_reader.py` expects tabs `02_CSRs_Catalog`, `03_CAL_Allocations`, `04_Threat_Mapping`; cs-concept-builder actually emits `05_CSR_Catalog`, `06_CAL_Allocation`, and has no threat-mapping tab. Additionally the reader keys on header cell `CSR_ID` while the concept writes `CSR ID` (space), and the column semantics differ (reader wants Title/Description/TARA_Level/Allocated_Nodes; concept emits Parent CSG/CAL/Branch/Node ID/Requirement Text/Verification Method). Net effect: fed a real cs-concept-builder workbook, the reader silently returns zero CSRs and zero CAL allocations — the concept→architecture hand-off is non-functional today. NOT fixed this run: the repair requires rewriting the reader's field mapping (tara_level vs CAL, allocated nodes vs per-CSR node) and touching downstream generator logic — a coordinated refactor, not a small polish. SKILL.md's "Step 1" also documents the wrong tab names and should be corrected in the same change.
- **[med, cross-skill] `cs-concept-builder/scripts/generate_cs_concept.py` ends with 3,361 trailing NUL bytes** (single run at EOF, offset 15753). The file still parses and runs, but this is file corruption from a bad write and will trip strict linters/diff tools. Fix is trivial (truncate) but belongs to cs-concept-builder, not today's target — deferred to a future POLISH pick.

**Suggested edits:**
1. Rewrite `cs_concept_reader.py` against the real cs-concept-builder contract: tabs `05_CSR_Catalog` (headers row 2: CSR ID / Parent CSG / CAL / Branch / Node ID / Requirement Text / Verification Method) and `06_CAL_Allocation` (Node ID / Node Name / Type / Inherited CAL / CSR Count); drop or make optional the `04_Threat_Mapping` read; map CAL→tara_level or rename the field.
2. Update SKILL.md Step 1 tab list to match.
3. Add a chain smoke test (concept sample → architecture) to examples/ once 1–2 land.
4. Separately: strip trailing NULs from cs-concept-builder's generator and re-zip.

**Severity:** high (silent upstream chain-break; builder itself is functionally sound in isolation).

**Applied this run:** none to the .skill file — the only defects found require coordinated cross-skill changes, which are out of scope for a small polish. Findings logged here and in AUTONOMOUS_LOG follow-ups.
