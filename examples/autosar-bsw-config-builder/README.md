# autosar-bsw-config-builder — Example

**What this skill produces:** An AUTOSAR Classic Basic Software configuration workbook per AUTOSAR
R22-11 — 15 tabs, verified against the generator on 2026-08-25: `Title`, `Document Control`,
`ECU-C Top-Level`, `Bus Interfaces`, `MCAL Module Inventory`, `ECU Abstraction Inventory`, `Service Layer Modules`,
`Complex Drivers`, `Module Parameters`, `Post-Build Variants`, `Memory Map`, `Schedule Tables`,
`Inter-Module Dependencies`, `Validation Rules`, `References`. It captures the middleware stack for
one ECU across all four AUTOSAR Classic layers, together with module parameters, OS task scheduling
and the ECU-C top-level target definition.

**Typical input shape:** A single JSON file — `examples/sample_input.json` ships inside the skill
archive as a worked Body Control Module example. Keys: `item` (name, abbr, project, doc_id,
revision, date, author, approver, company); `ecu_target` (microcontroller, vendor, memory_ram_kb,
memory_rom_kb); `bus_interfaces[]` (name, type, baudrate, channels); `mcal_modules[]` (id, name,
configured_for_target); `ecu_abstraction[]` (id, name, depends_on); `service_layer[]` (id, name,
critical); `complex_drivers[]` (id, name, purpose); `module_parameters[]` (module_id, parameter,
value, post_build_variant); `memory_layout[]` (region, start_address, size_kb, purpose); and
`os_tasks[]` (id, name, period_ms, priority).

**Expected output:** `bsw_config.xlsx` (or whatever second argument you pass). Twelve of the fifteen
tabs populate from input; three (`Inter-Module Dependencies`, `Validation Rules`, `Document Control`
change history) are header-only scaffolds the analyst completes by hand — they have no corresponding
input field. Two more tabs, `Memory Map` and `Bus Interfaces`, populated as of 2026-08-25; see the
resolved caveat below.

**Sample I/O:**

```bash
python scripts/generate_bsw.py examples/sample_input.json BCM-bsw.xlsx
```

prints `Generated BCM-bsw.xlsx` — a Body Control Module on an Infineon TC397XX with 8 MCAL modules,
3 ECU Abstraction modules, 8 Service Layer modules (5 marked critical), 2 complex drivers, 5 module
parameters across two post-build variants, 3 bus interfaces (2x CAN, 1x LIN), 4 memory regions and
5 OS tasks from 1 ms to 100 ms. Running the paired reviewer against that file yields 9 checks with
**all 7 Mandatory checks rated FC** and a Summary dashboard, verified 2026-08-25.

**Resolved caveat — the silent input drop is fixed ([#54](https://github.com/jherrodthomas/automotive-skills-suite/issues/54), 2026-08-25):**
Before 2026-08-25, `memory_layout` and `bus_interfaces` were documented in the generator's schema
and accepted without error but **never reached the workbook**, and the drop laundered into a passing
audit — the reviewer's `check_memory_allocation` (C005, Mandatory) rated supplied data
`NA — "No memory regions defined"`. Both now populate: `memory_layout` fills `Memory Map`, and
`bus_interfaces` fills the new `Bus Interfaces` tab. `Post-Build Variants` also populates now, rolled
up from `module_parameters[].post_build_variant`. If you generated a workbook with an older copy of
this skill, regenerate rather than trusting its Memory Map or its C005/C006 ratings.

**Paired reviewer:** `autosar-bsw-config-checklist-reviewer` — it resolves tabs by **exact name**
(`wb["MCAL Module Inventory"]`) and reads fixed column positions starting at row 3. Renaming a tab
or reordering a column breaks the probe silently and must be done in the same commit. The tab-name
contract was re-audited on 2026-08-25 after the `Bus Interfaces` insert — 46 assertions, 16 chains,
0 breaks; the probe resolves by name, not index, so inserting a tab is safe. C006
(`check_nvm_alignment`) now resolves the Module ID to a module *name* through the inventory tabs and
fires correctly. The new `Bus Interfaces` tab is currently written but not audited by any check.
