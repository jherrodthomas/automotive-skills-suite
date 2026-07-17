# aspice-process-evidence-builder — Example

**What this skill produces:** An ASPICE process evidence workbook xlsx (10 tabs) collecting assessment-ready evidence for one process — process identification, base-practice and generic-practice evidence rows, work product inventory, and performance/quantitative/improvement sections for CL2+ ratings.

**Typical input shape:** A JSON per `scripts/generate_aspice_evidence.py` giving the process ID/name, per-BP and per-GP evidence entries (artifact, location, date), and work products (see `examples/sample_evidence_input.json`, which covers SWE.1 with 2 BPs and 1 GP).

**Expected output:** `<process>-evidence.xlsx` — 10-tab workbook where tabs 00–05 (process info, BP evidence, GP evidence, work products) are fully populated from the JSON; a JSON status summary is printed on completion.

**Sample I/O:** Input the bundled SWE.1 sample → Output an evidence workbook with the 2 BP evidence rows and 1 GP row placed, work products inventoried, and sheet names all within Excel's 31-char limit.

**Run:** Trigger by phrasing, e.g. "Collect the SWE.1 evidence pack for the assessment" — then `python scripts/generate_aspice_evidence.py <input.json> <output.xlsx>`.

**Known limitations (logged 2026-07-15, see `docs/skill-polish-log/aspice-process-evidence-builder.md`):** tabs 06/08 (performance records, improvement history) are header-only scaffolds and tab 07 (quantitative metrics) is hard-coded — CL3+ data requested by SKILL.md Step 4 is not yet plumbed.

**Paired reviewer:** `aspice-process-evidence-checklist-reviewer` — audits evidence completeness per BP/GP and work-product traceability.
