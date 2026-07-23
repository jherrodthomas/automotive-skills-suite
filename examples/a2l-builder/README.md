# a2l-builder — example

**What this skill produces:** 13-tab ASAM MCD-2 MC (A2L) calibration description authoring workbook (Title, Document Control, ECU Identity, Characteristic Catalog, Measurement Catalog, Conversion Methods, Axis Descriptions, Record Layouts, Function Hierarchy, Group Hierarchy, Memory Address Mapping, Validation Rules, References).
**Typical input shape:** JSON with `item`, `ecu_identity`, `memory_map`, `characteristics`, `measurements`, `conversion_methods`, `axes` (`points` may be a breakpoint list or an int count), `records`, `functions`, `groups`.
**Expected output:** `<name>.xlsx`, styled navy/light-blue, one catalog row per input entry.
**Sample I/O:** `python scripts/generate_a2l.py input.json output.xlsx` — smoke input with 1 characteristic / 1 measurement / 1 axis yields all 13 tabs populated.
