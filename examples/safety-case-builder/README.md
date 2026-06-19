# safety-case-builder — Example

**What this skill produces:** An ISO 26262-2 Safety Case xlsx that integrates evidence from all prior phases (HARA, FSC, TSC, FMEDA, SW/HW safety requirements) into a structured Goal Structuring Notation (GSN) argument that the item is acceptably safe, with traceability matrices, metric summaries, confirmation-review status, and signoff blocks.

**Typical input shape:** Upstream work-product references (HARA, FSC, TSC, FMEDA, SW/HW requirement workbooks), the safety-goal-to-evidence mapping, ASIL targets, confirmation-review records, and the assessment scope.

**Expected output:** `<item>-safety-case.xlsx` — multi-tab workbook with the GSN claim/argument/evidence structure, a safety-goal-to-evidence traceability matrix, an aggregated metric summary (SPFM/LFM/PMHF roll-up), confirmation-review status, and the assessment-ready signoff page.

**Sample I/O:** Input the concept-phase package for an electric power-steering ECU (1 HARA, 1 FSC, 1 TSC, 1 FMEDA) → Output `EPS-safety-case.xlsx` with 4 top-level safety-goal claims, a 26-row evidence traceability matrix, and confirmation-review status per work product.

**Run:** Trigger by phrasing, e.g. "Assemble the safety case for the EPS ECU from the concept-phase work products".
