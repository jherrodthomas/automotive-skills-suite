# Polish log — autosar-swc-builder.skill

## 2026-05-21 (autonomous POLISH run, W21 target #3 / issue #8)

**Domain:** autosar · **Severity:** med · **Outcome:** small fix applied

### What's good
- SKILL.md is well structured: clear Inputs (JSON schema), a 13-tab Output
  contract, and a `/autosar-swc-builder` command stub.
- Correctly anchored to a concrete standard revision (AUTOSAR R22-11) and scoped
  to a single application / sensor-actuator component — a sensible unit of work.
- Output tabs already enumerate sender-receiver, client-server and mode-switch
  interfaces separately, so the workbook structure matched the DoD before edits.

### What to fix
1. **Weak trigger sentence (med).** The frontmatter `description` ended with the
   boilerplate "Use this skill when the user mentions autosar swc builder." —
   the same low-value self-referential trigger seen across other builders. It
   gave the classifier nothing to match beyond the literal skill name.
2. **Classic vs Adaptive not disambiguated (med).** The description said
   "AUTOSAR Classic SWC" but never told the model (or the reader) that the
   Adaptive Platform equivalent is a different skill. Risk: this skill fires on
   Adaptive-platform requests it cannot serve.
3. **Port interface types not surfaced in triggers (low).** sender-receiver vs
   client-server appeared only in the Inputs section, not in the trigger
   surface — so a user asking by interface type would not reliably route here.

### Edits applied this run
- Rewrote the `description` (now 723 chars, well under the 1024 limit):
  explicitly says "Classic Platform ... only", points Adaptive traffic to
  `autosar-adaptive-app-builder`, and expands triggers to name port-interface
  types, RTE events, ARXML SWC output, and the casual phrasing "define an
  AUTOSAR component".
- Updated the `## Usage` trigger line to match, with the same Classic-vs-Adaptive
  redirect.
- Body prose (Inputs / Output / Commands) left untouched — no refactor.

### Deferred (not done this run)
- The body still has one residual "AUTOSAR Classic SWC" line that does not carry
  the Adaptive redirect; cosmetic only, low value, left for a future pass.
- Worth checking the Adaptive counterpart (`autosar-adaptive-app-builder`) so the
  two descriptions cross-reference each other symmetrically — captured as a W22
  follow-up candidate.
