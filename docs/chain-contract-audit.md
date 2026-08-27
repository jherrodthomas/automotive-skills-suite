# Chain-contract audit — builder-to-builder xlsx handoffs

_Generated 2026-08-27 by `scripts/chain_contract_audit.py` (read-only; modifies no `.skill` file)._

Scope is builder-to-builder reads only, per the W34 descope of [#46](https://github.com/jherrodthomas/automotive-skills-suite/issues/46). Builder-to-reviewer pairs are excluded: a reviewer ships with the builder it reviews, so the two cannot drift apart the way #43 did.

## Summary

- Builders scanned: **76**
- Cross-skill reader scripts found: **15** (in 13 skills)
- Declared chains audited: **16**
- Sheet-name assertions checked: **46**

| Verdict | Count |
|---|---|
| MATCH | 41 |
| ALIAS | 4 |
| FALLBACK | 1 |
| SELF-AMBIG | 0 |
| UNVERIFIABLE | 0 |
| BREAK | 0 |

**No confirmed BREAKs.** Every hard-coded tab name a builder expects from another builder is a name that builder actually emits.

## Chains audited

| Reader | Upstream | Assertions | Worst verdict |
|---|---|---|---|
| `cs-architecture-builder` | `cs-concept-builder` | 7 | ALIAS |
| `cs-concept-builder` | `cs-goals-builder` | 1 | MATCH |
| `cs-goals-builder` | `tara-builder` | 2 | MATCH |
| `fmeda-builder` | `tsc-builder` | 2 | ALIAS |
| `fsc-builder` | `hara-builder` | 2 | MATCH |
| `hsi-builder` | `tsc-builder` | 1 | MATCH |
| `hw-architecture-builder` | `tsc-builder` | 5 | MATCH |
| `hw-safety-reqs-builder` | `tsc-builder` | 3 | MATCH |
| `safety-case-builder` | `fmeda-builder` | 3 | MATCH |
| `safety-case-builder` | `fsc-builder` | 2 | MATCH |
| `safety-case-builder` | `hara-builder` | 2 | FALLBACK |
| `safety-case-builder` | `tsc-builder` | 2 | MATCH |
| `sw-arch-builder` | `tsc-builder` | 5 | MATCH |
| `sw-fmea-builder` | `tsc-builder` | 1 | MATCH |
| `sw-sr-builder` | `tsc-builder` | 4 | MATCH |
| `tsc-builder` | `fsc-builder` | 4 | MATCH |

## Findings

| Verdict | Reader | Script:line | Function | Upstream | Expected tab | Note |
|---|---|---|---|---|---|---|
| FALLBACK | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:24 | `read_hara` | `hara-builder` | `05_Safety_Goals` | alternative branch; sibling matches |
| ALIAS | `cs-architecture-builder` | `scripts/cs_concept_reader.py`:23 | `(module constant)` | `cs-concept-builder` | `02_CSRs_Catalog` | declared legacy alias; preferred name matches |
| ALIAS | `cs-architecture-builder` | `scripts/cs_concept_reader.py`:24 | `(module constant)` | `cs-concept-builder` | `03_CAL_Allocations` | declared legacy alias; preferred name matches |
| ALIAS | `cs-architecture-builder` | `scripts/cs_concept_reader.py`:25 | `(module constant)` | `cs-concept-builder` | `04_Threat_Mapping` | declared legacy alias; preferred name matches |
| ALIAS | `fmeda-builder` | `scripts/tsc_reader.py`:29 | `(module constant)` | `tsc-builder` | `05_Safety_Mechanisms_From_TSC` | declared legacy alias; preferred name matches |
| MATCH | `cs-architecture-builder` | `scripts/cs_concept_reader.py`:23 | `(module constant)` | `cs-concept-builder` | `05_CSR_Catalog` |  |
| MATCH | `cs-architecture-builder` | `scripts/cs_concept_reader.py`:24 | `(module constant)` | `cs-concept-builder` | `06_CAL_Allocation` |  |
| MATCH | `cs-architecture-builder` | `scripts/cs_concept_reader.py`:25 | `(module constant)` | `cs-concept-builder` | `02_CS_Goals_Echo` |  |
| MATCH | `cs-architecture-builder` | `scripts/cs_concept_reader.py`:26 | `(module constant)` | `cs-concept-builder` | `00_Title_Page` |  |
| MATCH | `cs-concept-builder` | `scripts/cs_goals_reader.py`:16, 17 | `read_cs_goals` | `cs-goals-builder` | `00_Title_Page` |  |
| MATCH | `cs-goals-builder` | `scripts/tara_reader.py`:47, 48 | `read_tara_xlsx` | `tara-builder` | `11_Cybersecurity_Goals` |  |
| MATCH | `cs-goals-builder` | `scripts/tara_reader.py`:51, 52 | `read_tara_xlsx` | `tara-builder` | `09_Risk_Determination` |  |
| MATCH | `fmeda-builder` | `scripts/tsc_reader.py`:29 | `(module constant)` | `tsc-builder` | `04_Safety_Mechanism_Catalog` |  |
| MATCH | `fsc-builder` | `scripts/generate_fsc.py`:203, 204 | `read_hara` | `hara-builder` | `00_Title_Page` |  |
| MATCH | `fsc-builder` | `scripts/generate_fsc.py`:218, 220 | `read_hara` | `hara-builder` | `13_Safety_Goals` |  |
| MATCH | `hsi-builder` | `scripts/tsc_reader.py`:28, 32 | `read_hsi_signals` | `tsc-builder` | `06_HSI_Specification` |  |
| MATCH | `hw-architecture-builder` | `scripts/tsc_reader.py`:27, 28 | `read_tsc` | `tsc-builder` | `00_Title_Page` |  |
| MATCH | `hw-architecture-builder` | `scripts/tsc_reader.py`:40, 41 | `read_tsc` | `tsc-builder` | `02_FSRs_From_FSC` |  |
| MATCH | `hw-architecture-builder` | `scripts/tsc_reader.py`:59, 60 | `read_tsc` | `tsc-builder` | `03_System_Architecture` |  |
| MATCH | `hw-architecture-builder` | `scripts/tsc_reader.py`:111, 112 | `read_tsc` | `tsc-builder` | `05_TSR_Catalog` |  |
| MATCH | `hw-architecture-builder` | `scripts/tsc_reader.py`:128, 129 | `read_tsc` | `tsc-builder` | `04_Safety_Mechanism_Catalog` |  |
| MATCH | `hw-safety-reqs-builder` | `scripts/tsc_reader.py`:26, 44 | `read_tsc` | `tsc-builder` | `05_TSR_Catalog` |  |
| MATCH | `hw-safety-reqs-builder` | `scripts/tsc_reader.py`:31, 32 | `read_tsc` | `tsc-builder` | `00_Title_Page` |  |
| MATCH | `hw-safety-reqs-builder` | `scripts/tsc_reader.py`:69, 70, 90, 91 | `read_tsc` | `tsc-builder` | `03_System_Architecture` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:22 | `read_hara` | `hara-builder` | `13_Safety_Goals` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:50, 51 | `read_fsc` | `fsc-builder` | `05_FSR_Catalog` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:65, 66 | `read_fsc` | `fsc-builder` | `06_ASIL_Allocation` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:90, 91 | `read_tsc` | `tsc-builder` | `05_TSR_Catalog` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:106, 107 | `read_tsc` | `tsc-builder` | `04_Safety_Mechanism_Catalog` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:131, 132 | `read_fmeda` | `fmeda-builder` | `07_SPFM_Calculation` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:140 | `read_fmeda` | `fmeda-builder` | `08_LFM_Calculation` |  |
| MATCH | `safety-case-builder` | `scripts/multi_xlsx_reader.py`:141 | `read_fmeda` | `fmeda-builder` | `09_PMHF_Calculation` |  |
| MATCH | `sw-arch-builder` | `scripts/tsc_reader.py`:28, 29 | `read_tsc` | `tsc-builder` | `00_Title_Page` |  |
| MATCH | `sw-arch-builder` | `scripts/tsc_reader.py`:41, 42 | `read_tsc` | `tsc-builder` | `02_FSRs_From_FSC` |  |
| MATCH | `sw-arch-builder` | `scripts/tsc_reader.py`:58, 59 | `read_tsc` | `tsc-builder` | `03_System_Architecture` |  |
| MATCH | `sw-arch-builder` | `scripts/tsc_reader.py`:104, 105 | `read_tsc` | `tsc-builder` | `05_TSR_Catalog` |  |
| MATCH | `sw-arch-builder` | `scripts/tsc_reader.py`:123, 124 | `read_tsc` | `tsc-builder` | `04_Safety_Mechanism_Catalog` |  |
| MATCH | `sw-fmea-builder` | `scripts/tsc_reader.py`:34, 35 | `read_tsc` | `tsc-builder` | `00_Title_Page` |  |
| MATCH | `sw-sr-builder` | `scripts/tsc_reader.py`:26, 44 | `read_tsc` | `tsc-builder` | `05_TSR_Catalog` |  |
| MATCH | `sw-sr-builder` | `scripts/tsc_reader.py`:31, 32 | `read_tsc` | `tsc-builder` | `00_Title_Page` |  |
| MATCH | `sw-sr-builder` | `scripts/tsc_reader.py`:69, 70 | `read_tsc` | `tsc-builder` | `03_System_Architecture` |  |
| MATCH | `sw-sr-builder` | `scripts/tsc_reader.py`:97, 98 | `read_tsc` | `tsc-builder` | `06_HSI_Specification` |  |
| MATCH | `tsc-builder` | `scripts/generate_tsc.py`:209, 227 | `read_fsc` | `fsc-builder` | `05_FSR_Catalog` |  |
| MATCH | `tsc-builder` | `scripts/generate_tsc.py`:214, 215 | `read_fsc` | `fsc-builder` | `00_Title_Page` |  |
| MATCH | `tsc-builder` | `scripts/generate_tsc.py`:248, 249 | `read_fsc` | `fsc-builder` | `06_ASIL_Allocation` |  |
| MATCH | `tsc-builder` | `scripts/generate_tsc.py`:267, 268 | `read_fsc` | `fsc-builder` | `03_System_Block_Diagram` |  |

## Pattern-scan readers (no fixed contract)

These iterate `wb.sheetnames` and match by substring instead of asserting exact tab names. They cannot break on a rename, and they are the shape the other readers should converge on.

| Reader | Script | Upstream |
|---|---|---|
| `cs-concept-builder` | `scripts/cs_goals_reader.py` | `cs-goals-builder` |
| `sw-fmea-builder` | `scripts/tsc_reader.py` | `tsc-builder` |
| `sw-hsis-builder` | `scripts/hsi_reader.py` | `hsi-builder` |

## Self-reads (excluded from the audit)

These load the reading skill's OWN output, not an upstream workbook, so they are not a cross-skill contract.

| Skill | Script | Function | Tab |
|---|---|---|---|
| `fsc-builder` | `scripts/fault_tree_renderer.py` | `trees_from_fsc_xlsx` | `04_FTA_Combined` |
| `fsc-builder` | `scripts/fault_tree_renderer.py` | `trees_from_fsc_xlsx` | `02_SGs_From_HARA` |

## Method and limits

Static analysis only — no workbook is generated and no generator is executed. Specifically, this audit answers *does the upstream emit a tab with this name*, not *does that tab have the columns the reader indexes into*. Column-layout drift is a real second failure mode and is NOT covered here.

Verdict definitions: **MATCH** upstream emits it · **ALIAS** declared legacy alternative whose preferred sibling matches · **FALLBACK** the same shape expressed as an `elif`/`or` branch · **SELF-AMBIG** the reading skill emits the same tab name and attribution came only from SKILL.md prose · **UNVERIFIABLE** upstream unresolved · **BREAK** nothing upstream emits it.
