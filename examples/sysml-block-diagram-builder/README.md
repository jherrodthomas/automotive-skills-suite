# sysml-block-diagram-builder — Example

**What this skill produces:** A SysML Block Definition Diagram (BDD) / Internal Block Diagram
(IBD) authoring workbook per OMG SysML 1.6 / 2.0 — 11 tabs, verified against the generator on
2026-08-27: `0_Title`, `1_Document_Control`, `2_Model_Header`, `3_Block_Catalog`,
`4_Property_Catalog`, `5_Part_Reference_Properties`, `6_Port_Catalog`, `7_Connector_Catalog`,
`8_Item_Flow_Catalog`, `9_Value_Type_Definitions`, `10_References`. Intended as a
Cameo / Rhapsody / Capella import staging sheet.

**Typical input shape:** A JSON model description passed with `--input` (schema in the
builder's `SKILL.md`; worked example in `sample_input.json` beside this README — a brake-by-wire
system with 5 blocks, 3 properties, 3 parts, 7 ports, 3 connectors, 3 item flows, 4 value types).
Top-level keys: `system_name`, `author`, `version`, `model_header`, `blocks`, `properties`,
`parts`, `ports`, `connectors`, `item_flows`, `value_types`, `references`. All optional.
Connector `source` / `target` are written `Block.port` and must name ports in `ports`.
Added 2026-09-02 (#57); before that the generator had no input path at all.

**Expected output:** `sysml_block_diagram.xlsx` (or whatever path you pass). 11 tabs; rows in
tabs 3–9 come entirely from your JSON. `--title` / `--author` override the JSON values. Run it
without `--input` and you get an empty template (headers only) plus a stderr warning.

**Sample I/O:**

```bash
python scripts/generate_sysml_block.py brake_by_wire.xlsx \
    --input examples/sysml-block-diagram-builder/sample_input.json
```

prints `Block diagram workbook saved to brake_by_wire.xlsx`. Block Catalog reads
`BrakeByWireSystem` / `PedalUnit` / `BrakeActuatorECU` / `WheelBrakeActuator` / `Driver`;
`2_Model_Header` reports `5 blocks, 7 ports, 3 connectors`.

**Paired reviewer:** `sysml-block-diagram-checklist-reviewer` — 15 machine checks (`BDD-01`
through `BDD-15`), 11 of which are verifiable and 4 of which return auto-suggest drafts for
manual confirmation. Against the sample-input build it reports 10 FC, 1 PC, 0 NO (verified 2026-09-02). The
PC is `BDD-07`, flagging the composition root `BrakeByWireSystem` as having no connector of its own.

The reviewer's probe resolves tabs by **name fragment or leading index** (`"block_catalog" in
name.lower() or name.startswith("3")`), so it survives tab reordering but not renaming — and
its data rows start at **row 4**. Any change to the header block position in this builder
silently empties the reviewer.

**`BDD-12` fixed 2026-09-02.** It now validates both connector ends against the Port Catalog.
A sample input with one target changed to a non-existent port returns `BDD-12 NO`.

**Domain note:** the other three sysml reviewers (`activity-diagram`, `requirement-diagram`,
`state-machine`) currently ship **zero** checks despite advertising 25 each, and emit a
title-only checklist. This block-diagram pair is the only sysml pair with a working reviewer.
