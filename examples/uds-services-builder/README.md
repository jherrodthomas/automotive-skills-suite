# uds-services-builder — Example

**What this skill produces:** An ISO 14229-1 UDS (Unified Diagnostic Services) catalog xlsx documenting supported services, sub-functions, DIDs (Data Identifiers), RIDs (Routine Identifiers), security access levels, session dependencies, and negative-response code (NRC) handling for an ECU.

**Typical input shape:** ECU identity (variant, supplier), supported service IDs (0x10, 0x11, 0x22, 0x27, 0x2E, 0x31, 0x34–0x37, 0x3E, 0x85, …), DID/RID inventory, security access seed/key scheme reference, session timing (P2/P2*), and tester-present cadence.

**Expected output:** `<ecu>-uds-catalog.xlsx` — multi-tab workbook (Services, DIDs, RIDs, Sessions, Security Access, NRC matrix, Timing, Traceability to UDS-TSR) with conditional formatting on unsupported NRCs.

**Sample I/O:** Input a body-control ECU diagnostic spec stub → Output `BCM-uds-catalog.xlsx` with 32 DIDs, 9 RIDs, and 4 security access levels populated.

**Run:** Trigger by phrasing, e.g. "Build a UDS services catalog for the BCM".
