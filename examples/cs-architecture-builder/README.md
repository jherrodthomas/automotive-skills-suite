# cs-architecture-builder — Example

**What this skill produces:** An ISO/SAE 21434 cybersecurity architecture workbook xlsx (12 tabs) refining a Cybersecurity Concept into an element-level security architecture — CS elements with CAL inheritance, CSR-to-element allocations, control selection from the bundled `references/cs_control_catalog.md`, and hand-off tabs for implementation.

**Typical input shape:** An upstream CS Concept workbook (e.g. a `cs-concept-builder` output) plus a JSON per `scripts/generate_cs_architecture.py` describing the architecture block diagram — elements, types, interfaces (see `examples/sample_cs_arch_input_esc.json`).

**Expected output:** `<item>-cs-architecture.xlsx` — 12-tab workbook; the ESC sample yields 7 CS elements and 14 CSR allocations with all sheet titles ≤31 chars.

**Sample I/O:** Input the ESC block-diagram sample plus a concept workbook → Output an architecture workbook allocating each CSR to concrete elements with per-element CAL and proposed controls.

**Run:** Trigger by phrasing, e.g. "Refine the ESC cybersecurity concept into an architecture" — then `python scripts/generate_cs_architecture.py <cs_concept.xlsx> <input.json> <output.xlsx>`.

**Known limitation (logged 2026-07-16, see `docs/skill-polish-log/cs-architecture-builder.md`):** `scripts/cs_concept_reader.py` expects tab names/headers that current `cs-concept-builder` output does not emit, so a real concept workbook currently reads back zero CSRs — the concept→architecture hand-off needs a coordinated reader rewrite. The builder is sound in isolation and its forward chain to the reviewer is exact (10/10 tab-name matches).

**Paired reviewer:** `cs-architecture-checklist-reviewer` — audits element coverage, CAL inheritance, and CSR allocation completeness.
