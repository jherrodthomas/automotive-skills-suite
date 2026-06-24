# communication-matrix-builder — polish log

## 2026-06-24 (autonomous POLISH pass)

**Severity:** low (description-quality fix applied)

**What's good**
- Clear scope: K-Matrix workbook spanning CAN / CAN FD / LIN / FlexRay / Automotive Ethernet, which is the realistic multi-bus picture.
- Body documents a concrete 12-tab structure and the tab list in "What you'll get" matches the stated 12-tab count (Title, Document Control, Topology, ECU Inventory, Message Catalog, Signal Catalog/ownership, Sender-Receiver, Bus Allocation, Gateway Routing, Signal Timing Budget, Validation Rules, References).
- Input contract is explicit (AUTOSAR extract / FIBEX / JSON) and the "How to use it" steps are actionable.
- Ships scripts (generate_kmatrix.py, recalc.py, office/soffice.py) and two reference docs (methodology, kmatrix_conventions) — good supporting material.

**What to fix**
- (FIXED this pass) The frontmatter `description` trigger clause was too narrow: "Use this skill when the user mentions communication matrix builder." It only fired on the exact phrase, while the body's "When to use" section lists a much richer trigger set. Broadened the description trigger clause to: K-Matrix, communication matrix, signal database, cross-bus signal mapping, ECU signal allocation, gateway routing specification, automotive network audit preparation. New description length 610 chars (well under the 1024 limit).
- (Future, not done) Body "When to use" still lists "vehicle network coordination" which the description omits; harmless, but the two lists could be reconciled in a later pass.
- (Future, not done) Consider stating explicitly whether the generated workbook does bus-load estimation, since a sibling skill (bus-load-*) may overlap — a one-line scope boundary would prevent confusion.

**Suggested edits (deferred)**
- Add a short "Out of scope" note pointing bus-load and gateway-routing deep dives to their dedicated skills.
- Add an example input JSON under examples/ (none present) so the reviewer skill has a sample to validate against.
