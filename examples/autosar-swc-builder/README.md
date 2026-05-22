# autosar-swc-builder — Example

**What this skill produces:** A 13-tab audit-ready xlsx AUTOSAR Classic Platform SWC (Software Component) specification workbook per AUTOSAR R22-11 — SWC type, port interfaces (sender-receiver, client-server, mode-switch, NV-data, parameter, trigger), internal behavior, runnables, RTE events, exclusive areas, and data types.

**Typical input shape:** A JSON file with SWC name, category (Application / Sensor-Actuator / Service / Complex Driver), ports, internal behavior, runnables, events, exclusive areas, and data type definitions.

**Expected output:** `<swc-name>-swc-spec.xlsx` — 13 tabs from Title and Document Control through Port Inventory, interface tabs, Runnable Catalog, Event Catalog, and References.

**Sample I/O:** Input `door-control-swc.json` (category: Application) → Output `door-control-swc-spec.xlsx` with Port Inventory and Runnable Catalog tabs populated.

**Run:** `/autosar-swc-builder <SWC name> <category>`
