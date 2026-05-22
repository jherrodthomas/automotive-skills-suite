# dbc-builder — Example

**What this skill produces:** An 11-tab audit-ready xlsx DBC (CAN database) specification — ECU inventory, message catalog, signal catalog with bit-level encoding, signal-group/multiplexing, value tables, network topology, and validation rules per ISO 11898 and CAN 2.0A/2.0B.

**Typical input shape:** A JSON definition with project context, ECU list, messages (identifier, DLC, cycle time, sender, frame type), signals (bit start/length, byte order, scale/offset, min/max, unit, receivers), value tables, and multiplexing structure.

**Expected output:** `<network>-dbc-spec.xlsx` — 11 tabs from Title Page through References; the generator also prints a JSON summary with message, signal, and ECU counts.

**Sample I/O:** Input `examples/sample_input_can.json` → Output `powertrain-can-dbc.xlsx`.

**Run:** `python scripts/generate_dbc.py <input.json> <output.xlsx>`
