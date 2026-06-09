# Polish log — sotif-analysis-builder

## 2026-06-09 (W24 POLISH, first pass)

**Tracking issue:** #20 (W24 polish target, label `sotif`)
**Mode:** POLISH · **Severity:** low (no blocking issues found)
**In-place fix applied:** none required (see rationale)

### What's good
- **Frontmatter is valid and complete.** Required fields `name` and `description`
  both present; `description` is 619 chars, comfortably under the 1024-char ceiling.
  The description correctly frames SOTIF as distinct from ISO 26262 random-HW faults,
  which is the single most important triggering distinction for this skill.
- **Body is coherent and self-consistent.** The "Output structure (12 tabs)" header
  matches the table exactly (rows 00–11 = 12 tabs). The tab inventory in the body
  lines up with the tab list implied by the description.
- **Schema/example/generator are aligned.** Example input
  `examples/sample_sotif_input_aeb.json` has top-level keys
  `[system, functions, odd_definition, performance_limitations,
  functional_insufficiencies, triggering_conditions, hazardous_behaviors,
  scenarios, acceptance_criteria]` and every one is consumed by
  `scripts/generate_sotif_analysis.py` via a matching `data.get(...)`. No orphan
  fields, no fields the generator reads that the example omits.
- **Scripts are syntactically sound.** `generate_sotif_analysis.py` and `recalc.py`
  both parse without error. No `TODO`/`FIXME`/`placeholder`/`lorem` markers anywhere
  in the archive.
- **Correct cross-reference.** "When to use" routes random-HW-failure hazards to
  hara-builder, reinforcing the SOTIF-vs-FuSa boundary.

### What to fix
- Nothing blocking. The skill is in good health.

### Suggested (non-blocking) edits for a future pass
- **Quadrant terminology consistency (cosmetic):** the body uses both the standard
  ISO 21448 4-quadrant labels and the prose "Known/Unknown × Safe/Unsafe". These
  agree, but a one-line legend near tab 09 would remove any ambiguity for new users.
- **Acceptance-criterion singular/plural:** prose refers to "Acceptance criterion"
  (tab 10) while the input key is `acceptance_criteria` (plural). The generator
  already keys on the plural, so this is purely descriptive — worth a one-word
  harmonization only if the file is opened for another reason.
- **Reference freshness:** `references/iso21448_clauses.md` cites the §6 concept /
  §5 scenario taxonomy structure; confirm against the current ISO 21448:2022 clause
  numbering on a future content pass (low priority — structure is unchanged).

### Rationale for no in-place change
Per POLISH discipline, edits are limited to small, obvious, unambiguous fixes
(typo, over-length description, missing required frontmatter). None of those
conditions are present here. The suggestions above are content-judgement calls,
not mechanical fixes, so they are logged for a future deliberate pass rather than
applied blind. A small honest commit beats a wrong one.

### Net assessment
Healthy skill. The 🟡 staleness flag in STATUS.md is **date-based, not
quality-based** — the file simply hasn't been edited since 2026-05-01. This review
satisfies the standing SOTIF-coverage mandate (SOTIF was the only zero-touch domain
in the May KPI); the domain has now had a documented quality pass.
