# control-plan-builder — Example

**What this skill produces:** An AIAG-aligned Production Control Plan xlsx (13 tabs) covering the header block, process reference, the main control-plan worksheet, color-coded special characteristics (Critical / Significant / Standard), the measurement-system-analysis plan, a reaction-plan library, SPC method allocation, a mistake-proofing catalog, the audit plan, and PFMEA linkage — IATF 16949-compliant.

**Typical input shape:** Part/program identification, the process flow (operations in sequence), product and process special characteristics, gage and SPC method assignments, reaction plans, and the upstream PFMEA reference.

**Expected output:** `<part>-control-plan.xlsx` — multi-tab workbook with each process step mapped to its characteristics, control method, sample size/frequency, and reaction plan, plus MSA and mistake-proofing tabs cross-referenced to the PFMEA.

**Sample I/O:** Input a 7-station injection-molded housing process with 15 sample characteristics → Output `Housing-control-plan.xlsx` with 15 controlled characteristics (3 Critical color-coded red), an MSA plan, and PFMEA linkage rows.

**Run:** Trigger by phrasing, e.g. "Build a production control plan for the injection-molded housing line".
