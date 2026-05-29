# hara-builder — Example

**What this skill produces:** An ISO 26262-3 HARA (Hazard Analysis and Risk Assessment) xlsx covering item description, operational situations, malfunction guide-word brainstorm, hazard identification, S/E/C classification, ASIL determination, safety goals, and safe-state declaration — audit-ready for concept-phase assessment.

**Typical input shape:** Item definition (ECU/function, vehicle context), driving scenarios, malfunction guide words (loss-of, unintended, stuck, incorrect), and supporting evidence references (driver studies, fleet data).

**Expected output:** `<item>-hara.xlsx` — multi-tab workbook (Item, Operating Situations, Hazards, S/E/C, ASIL Matrix, Safety Goals, Safe States, Traceability) with ASIL auto-computed from S/E/C lookup.

**Sample I/O:** Input an Electronic Stability Control item definition stub → Output `ESC-hara.xlsx` with 14 hazards classified and 4 safety goals derived.

**Run:** Trigger by phrasing, e.g. "Build a HARA for a new ECU project — Electronic Stability Control".
