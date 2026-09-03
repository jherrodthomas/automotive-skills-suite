# mbse-system-context-builder — polish log

## 2026-09-03 (autonomous POLISH, #58 — first pass, first mbse entry)

**Scope:** `mbse-system-context-builder.skill` + `mbse-system-context-checklist-reviewer.skill`.
Verified by execution: builder run from a written ACC sample input and from `{}`; reviewer run
against both outputs, full 6-row rating spread read back.

### What's good
- The builder **does** consume its JSON input — `actors`, `interfaces`, `use_cases`, `scenarios`
  all land in their tabs with the right columns. Unlike the sysml set (#57) and
  `traceability-matrix` (#52) this is a real generator, not a placeholder emitter.
- All 12 emitted sheet names are Excel-legal (longest `Assumptions and Constraints`, 27 chars).
- Frontmatter valid on both halves; descriptions 404 / 291 chars.
- `recalc.py` 5,782 bytes at the single repo hash `cf419d15…` (both halves; 152/152 repo-wide).
- The 6 reviewer checks that exist are real: they read the probe and change rating with the data
  (`{}` build → 4 NO / 1 PC / 1 FC; sample build → 5 FC / 1 PC).
- **Chain-scan row resolved:** the 2026-09-01 scan left `Stakeholder Needs` unresolved because the
  builder creates it via a dynamic `create_sheet(sheet_name)`. Executed: the tab exists under
  exactly that name, the probe finds it. Not a break.

### What was wrong — and fixed (one line)
- **Reviewer crashed on every input** — `AttributeError: 'list' object has no attribute 'items'`
  at `dashboard.py:138`. `generate_checklist.py` builds `results` as a flat list but the shared
  `build_dashboard` expects `{tab: [(check, result)]}`. Same crash class as #56. Fixed by wrapping:
  `build_dashboard(ws, {"CR": results})`. The identical line exists in the other two mbse
  reviewers (`mbse-model-architecture`, `mbse-requirements-allocation`) and both crashed identically
  on their builders' `{}` output; same one-line fix applied and verified on all three. Before today,
  **the entire mbse domain had zero working reviewers.**

### What to fix — filed here, not fixed (severity)
1. **HIGH — 7 of 12 tabs are placeholders** (`System Boundary Definition`, `Stakeholder Needs`,
   `Assumptions and Constraints`, `Validation Rules`, `References`, `Document Control`,
   `System Identification`) each containing one cell `"<name> (Placeholder)"`. Input keys
   `stakeholder_needs` and `boundary` are silently discarded. Consequence: reviewer `CR05`
   (stakeholder needs) can never pass on this builder's own output, and `CR06` passes on
   *presence of a tab holding one placeholder string*.
2. **MED — SKILL.md advertises 11 tabs, generator emits 12.** `Validation Rules` is emitted but
   not listed; it also does not belong in a system-context deliverable.
3. **MED — SKILL.md example contradicts the generator.** The example's use case carries
   `"actors": ["A01","A02"]`; the generator reads `primary_actor` (string). Following the doc
   yields a blank Primary Actor column.
4. **MED — Reviewer SKILL.md advertises "25+ checks" in 7 categories; 6 ship.** Standing item 9.
5. **LOW — `CR05` has no NO branch** (zero needs → PC), so an empty Needs tab is "partially
   compliant".
6. **LOW — obligation vocabulary mismatch.** Checks use `Must`/`Should`; `dashboard.py` counts
   major issues as `NO and obligation == "Shall"`. `n_major` is therefore always 0 and the
   `REJECTED` status can never fire — a `{}` workbook with 4 NO reports `CONDITIONAL APPROVAL`.
   Same latent bug in the two sibling mbse reviewers (not verified beyond grep).
7. **LOW — probe reads `Title!B5` as `author`;** the builder writes the creation date there.

### Suggested edits (for the W37 table)
- Wire `stakeholder_needs`, `boundary`, `assumptions`, `references`, `document_control` into their
  tabs — the input shape in `examples/mbse-system-context-builder/sample_input.json` already
  carries the first two. Drop `Validation Rules` or document it.
- Fix the SKILL.md example (`primary_actor`) and tab count in the same change.
- Reviewer: either author the missing ~19 checks or downgrade SKILL.md to "6 checks".
- Repo-wide: grep for `obligation == "Shall"` in `dashboard.py` vs `Must` in checks — likely
  wider than mbse.
