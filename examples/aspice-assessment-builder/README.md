# aspice-assessment-builder — Example

**What this skill produces:** An audit-ready ASPICE PAM v3.1/v4.0 process assessment xlsx (13 tabs) covering the title page, document control, assessment scope, the process reference model, the process assessment model, base-practice and generic-practice assessments, capability-level determinations (CL0–CL5), a strengths/weaknesses analysis, and assessor sign-off blocks.

**Typical input shape:** Assessment metadata (project, document ID, date, assessor / lead assessor, target ASPICE version), the process areas in scope (e.g. SYS.1–5, SWE.1–6, SUP.1/8/9/10, MAN.3/5, ACQ.4), and per-process base/generic practices each rated N/P/L/F (Not / Partially / Largely / Fully achieved) with evidence references.

**Expected output:** `<project>-aspice-assessment.xlsx` — multi-tab workbook where each in-scope process carries its rated indicators, the process-attribute ratings aggregate into a capability level, and the summary tabs roll up strengths, weaknesses, and the assessor sign-off.

**Sample I/O:** Input SWE.1–SWE.6 + SYS.2/SYS.3 in scope, each base practice rated against project evidence → Output `BrakeECU-aspice-assessment.xlsx` showing SWE.1 at CL2, SWE.3 at CL1 (one P-rated PA1.1 indicator), and a weaknesses tab listing the gap indicators driving each downrating.

**Run:** Trigger by phrasing, e.g. "Run an ASPICE v3.1 assessment for the brake ECU software processes" — then `python scripts/generate_aspice_assessment.py <input.json> <output.xlsx>`.

**Paired reviewer:** `aspice-assessment-checklist-reviewer` — audits this workbook for rating consistency, evidence completeness, and capability-level derivation.
