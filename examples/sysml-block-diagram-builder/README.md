# sysml-block-diagram-builder — Example

**What this skill produces:** A SysML Block Definition Diagram (BDD) / Internal Block Diagram
(IBD) authoring workbook per OMG SysML 1.6 / 2.0 — 11 tabs, verified against the generator on
2026-08-27: `0_Title`, `1_Document_Control`, `2_Model_Header`, `3_Block_Catalog`,
`4_Property_Catalog`, `5_Part_Reference_Properties`, `6_Port_Catalog`, `7_Connector_Catalog`,
`8_Item_Flow_Catalog`, `9_Value_Type_Definitions`, `10_References`. Intended as a
Cameo / Rhapsody / Capella import staging sheet.

**Typical input shape:** **None — and this is the thing to know before using it.** The
generator accepts only `output`, `--title` and `--author`. There is no JSON input, no fixture,
and no way to supply your own blocks, ports, connectors or item flows. Every content row in
tabs 3 through 9 is hard-coded placeholder text (`System`, `Subsystem_A`, `Subsystem_B`,
`Component_A1`, `ExternalActor`). Treat the output as a **pre-filled template you edit by
hand**, not as a generated model. 71 of the repo's 76 builders read a JSON input file; this one
is one of five that do not. See the polish log.

**Expected output:** `sysml_block_diagram.xlsx` (or whatever path you pass). 11 tabs, 5
placeholder blocks, 4 ports, 2 connectors, 2 item flows, 3 value types. Your `--title` reaches
tabs 0 and 1; your `--author` reaches tab 1. Nothing else you supply reaches the file.

**Sample I/O:**

```bash
python scripts/generate_sysml_block.py brake_by_wire.xlsx \
    --title "Brake-by-Wire System" --author "J. Herrod"
```

prints `Block diagram workbook saved to brake_by_wire.xlsx`. The cover reads
"Brake-by-Wire System"; the Block Catalog reads `System` / `Subsystem_A` / `Subsystem_B` /
`Component_A1` / `ExternalActor`.

**Paired reviewer:** `sysml-block-diagram-checklist-reviewer` — 15 machine checks (`BDD-01`
through `BDD-15`), 11 of which are verifiable and 4 of which return auto-suggest drafts for
manual confirmation. Against this builder's unedited output it reports 10 FC, 1 PC, 0 NO. The
PC is `BDD-07`, correctly flagging `Subsystem_B` and `ExternalActor` as connected to nothing.

The reviewer's probe resolves tabs by **name fragment or leading index** (`"block_catalog" in
name.lower() or name.startswith("3")`), so it survives tab reordering but not renaming — and
its data rows start at **row 4**. Any change to the header block position in this builder
silently empties the reviewer.

**Known gap — do not trust `BDD-12`.** It validates connector *source* ports only and never
looks at target ports. This builder's own placeholder data references two target ports that do
not exist in the Port Catalog (`Subsystem_A.data_out`, `Component_A1.service_req`) and `BDD-12`
still returns FC at High confidence. Verify connector targets by hand until that check is
fixed. Recorded in the polish log, not yet repaired.

**Domain note:** the other three sysml reviewers (`activity-diagram`, `requirement-diagram`,
`state-machine`) currently ship **zero** checks despite advertising 25 each, and emit a
title-only checklist. This block-diagram pair is the only sysml pair with a working reviewer.
