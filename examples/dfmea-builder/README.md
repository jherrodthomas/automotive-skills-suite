# dfmea-builder — Example

**What this skill produces:** An AIAG-VDA aligned Design FMEA xlsx with structure analysis, function analysis, failure analysis (failure modes/effects/causes), Severity/Occurrence/Detection ratings, Action Priority (AP) classification, and recommended actions with owner/due tracking.

**Typical input shape:** Item under analysis (system/subsystem/component), interface diagram, function tree, design assumptions, prior-art failure history (8D / warranty), and team roster.

**Expected output:** `<item>-dfmea.xlsx` — multi-tab workbook (Structure, Function, Failure Net, DFMEA Worksheet, Action Plan, Risk Trend) with AP auto-classified from the AIAG-VDA S×O×D matrix.

**Sample I/O:** Input a wiper motor assembly definition with 6 functions → Output `WiperMotor-dfmea.xlsx` with 22 failure modes and 9 high-AP actions logged.

**Run:** Trigger by phrasing, e.g. "Build a DFMEA for the wiper motor assembly".
