# communication-matrix-builder — Example

**What this skill produces:** An audit-ready K-Matrix communication matrix xlsx (12 tabs) covering all bus traffic across CAN, CAN FD, LIN, FlexRay, and Automotive Ethernet — title and document-control tabs, vehicle network topology, ECU inventory with bus memberships, a consolidated message catalog, a signal catalog and ownership matrix, the sender-receiver matrix, bus-allocation specs, gateway-routing requirements, a signal timing-budget analysis, and validation rules.

**Typical input shape:** An AUTOSAR system extract (XML/FIBEX) or JSON describing ECUs, buses, messages, and signals; the vehicle architecture (e.g. classical CAN, CAN FD backbone with LIN clusters, Ethernet gateway); signal timing constraints; and gateway routing policies.

**Expected output:** `<vehicle>-kmatrix.xlsx` — multi-tab workbook mapping every signal to its owning ECU and bus, the sender-receiver relationships, gateway routes between segments, and per-signal timing budgets, with cross-checks and traceability.

**Sample I/O:** Input a CAN FD backbone with two LIN sub-clusters and an Ethernet gateway, ~40 messages → Output `Vehicle-kmatrix.xlsx` with a consolidated message catalog, a sender-receiver matrix, gateway-routing rows for the CAN↔Ethernet hops, and a timing-budget tab flagging any signal whose period exceeds its deadline.

**Run:** Trigger by phrasing, e.g. "Build the K-Matrix for our CAN FD backbone with LIN clusters" — then `python scripts/generate_kmatrix.py <input.json> <output.xlsx>`.

**Paired reviewer:** `communication-matrix-checklist-reviewer` — audits this workbook for signal-ownership uniqueness, gateway-route completeness, and timing-budget consistency.
