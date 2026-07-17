# ppap-package-builder — Example

**What this skill produces:** A complete AIAG PPAP submission package xlsx (22 tabs) integrating upstream artifact references (DFMEA, PFMEA, Control Plan, MSA, dimensional data, process capability) into an OEM-compliant submission — including the Part Submission Warrant (PSW) signoff block, submission level matrix (Levels 1–5), and per-element status tracking per IATF 16949 and the AIAG PPAP 4th edition baseline.

**Typical input shape:** A JSON per `scripts/generate_ppap_package.py` giving part/program identification (part number, revision, customer, submission level), the PSW declaration fields, and per-element evidence references (document IDs, dates, status) for the 18 PPAP elements.

**Expected output:** `<part>-ppap-package.xlsx` — 22-tab workbook with title page, document control, PSW, submission-level requirements matrix, one tab per evidence element, and an element status roll-up ready for customer signoff.

**Sample I/O:** Input `examples/sample_ppap_input_esc.json` (ESC ECU, Level 3 submission) → Output a package where the PSW carries the part and declaration data, the level matrix marks each element Submit/Retain per Level 3, and each element tab cross-references its upstream artifact.

**Run:** Trigger by phrasing, e.g. "Build the Level 3 PPAP package for the ESC ECU" — then `python scripts/generate_ppap_package.py <input.json> <output.xlsx>`.

**Paired reviewer:** `ppap-checklist-reviewer` (alias pairing per `docs/PAIRING_ALIASES.md`) — audits the package against AIAG PPAP 4th edition: doc quality, element compliance, and verification checks. Note: two known probe mismatches (tab 09 name length, Element-18 status tab) are logged in `docs/skill-polish-log/ppap-package-builder.md`.
