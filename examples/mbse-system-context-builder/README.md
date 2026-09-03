# mbse-system-context-builder — Example

**What this skill produces:** An MBSE system-context workbook (operational-analysis layer input
for ARCADIA / MagicGrid / STRATA). Verified against the generator on 2026-09-03: **12 tabs** —
`Title`, `Actor Inventory`, `External Interfaces`, `Use Case Catalog`, `Operational Scenarios`,
`System Boundary Definition`, `Stakeholder Needs`, `Assumptions and Constraints`,
`Validation Rules`, `References`, `Document Control`, `System Identification`. SKILL.md says 11;
`Validation Rules` is the extra. Only the first five tabs carry data today — the other seven are
single-cell placeholders (see the polish log).

**Typical input shape:** JSON passed as the first positional argument; worked example in
`sample_input.json` beside this README (an Adaptive Cruise Control context: 5 actors,
6 interfaces, 4 use cases, 3 scenarios, 5 stakeholder needs, a boundary block). Keys the
generator reads: `system.name`, `actors[]` (`id`,`name`,`type`,`description`), `interfaces[]`
(`id`,`from`,`to`,`direction`,`description`), `use_cases[]` (`id`,`name`,`primary_actor`,
`description` — **not** the `actors` list shown in SKILL.md), `scenarios[]`
(`id`,`name`,`description`). `stakeholder_needs` and `boundary` are accepted in the sample but
currently ignored by the generator.

**Expected output:** `<input>.xlsx` or the path you pass second. Rows in tabs 2–5 come
entirely from your JSON; an empty `{}` input yields headers only.

**Sample I/O:**

```bash
python scripts/generate_context.py examples/mbse-system-context-builder/sample_input.json acc_context.xlsx
```

prints `Workbook saved to acc_context.xlsx`. Actor Inventory reads `Driver` / `Lead Vehicle` /
`Brake System ECU` / `Powertrain ECU` / `Road Environment`.

**Paired reviewer:** `mbse-system-context-checklist-reviewer` — 6 machine checks `CR01`–`CR06`
(SKILL.md claims 25+). Against the sample build: **5 FC / 1 PC**, overall `CONDITIONAL APPROVAL`;
the PC is `CR05` (stakeholder needs) and it cannot pass until the builder populates that tab.
Against a `{}` build: 4 NO / 1 PC / 1 FC. Before 2026-09-03 the reviewer crashed on every input
(`dashboard.py` results-shape mismatch, fixed in #58).
