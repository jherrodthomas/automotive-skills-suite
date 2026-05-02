<div align="center">

# Automotive Engineering Suite

**152 installable Claude skills covering automotive safety, cybersecurity, quality, communication, diagnostics, AUTOSAR, calibration, MBSE, SysML, and V&V**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/Skills-152-blue)](skills/)
[![Standards](https://img.shields.io/badge/Standards-ISO%2026262%20%7C%2021434%20%7C%2021448%20%7C%20IATF%2016949%20%7C%20ASPICE%20%7C%20AUTOSAR%20%7C%20ISO%2014229%20%7C%20SysML-green)]()
[![Chain](https://img.shields.io/badge/Chain-Builder%20%2B%20Reviewer%20pairs-orange)]()
[![Made for](https://img.shields.io/badge/Made%20for-Claude-purple)](https://claude.com)

Every artifact-producing skill is paired with a confirmation reviewer. Reviewer outputs include a visual dashboard with KPI tiles, charts, and findings tables.

[Quickstart](#quickstart) · [The Chain](#the-chain) · [Skills](#skills) · [Standards](#standards-covered) · [Why](#why)

</div>

---

## What this is

A curated set of 76 builder skills + 76 matching confirmation reviewer skills that automate the structured xlsx deliverables required across the automotive engineering lifecycle:

- **ISO 26262** functional safety (concept → HW → SW → safety case)
- **ISO/SAE 21434** cybersecurity (TARA → goals → concept → architecture → IR plan)
- **ISO 21448 SOTIF** for ADAS/AV (analysis → triggering conditions → validation strategy)
- **AIAG-VDA quality** (APQP → DFMEA → PFMEA → Control Plan → PPAP)
- **Automotive SPICE** (assessment → gap analysis → improvement → evidence)
- **Communication protocols** (DBC / LDF / ARXML / FlexRay / Automotive Ethernet / K-Matrix / bus load / gateway routing)
- **Diagnostics** (UDS / DTC catalog / CDD / ODX / DEM)
- **AUTOSAR** (Classic SWC, Composition, BSW, RTE; Adaptive applications)
- **Calibration** (A2L / DCM / cross-supplier exchange)
- **MBSE** (ARCADIA model architecture, requirements allocation, system context)
- **SysML** (block diagram, requirement diagram, activity diagram, state machine)
- **V&V** (verification plan, validation plan, test case catalog, traceability matrix, V&V execution report)
- **Chain-integrated program management** (risk register, gate review, WP roll-up, change impact, lessons learned)
- **Continuous improvement** (8D, 5-Why, fishbone, MSA Gauge R&R, SPC)

Each skill is an installable `.skill` file. The chain is the moat — every downstream skill consumes the upstream skill's xlsx output as a stable file-format contract.

## Quickstart

1. Download the skill you need from `skills/` (or the full bundle from Releases)
2. Click "Save skill" in Cowork / Claude Desktop to install
3. Trigger by phrasing — every skill declares its triggering description in its frontmatter

```bash
# Example: install the HARA builder, then ask Claude
"Build a HARA for a new ECU project — Electronic Stability Control"
```

## The chain

```
Item Definition → Safety Plan → DIA
        ↓
      HARA → FSC → TSC
        ↓                ↓
   HW Lane         SW Lane
   ↓ ↓ ↓ ↓         ↓ ↓ ↓ ↓
HW-SR  Arch  HSI  FMEDA   SW-SR  Arch  FMEA  HSI
        ↓                ↓
            Safety Case (capstone)

Parallel chains:
TARA → CS Goals → CS Concept → CS Architecture → IR Plan + Secure Coding   (ISO/SAE 21434)
SOTIF Analysis → Triggering Conditions → Validation Strategy                  (ISO 21448)
APQP Plan → DFMEA → PFMEA → Control Plan → PPAP Package                       (IATF 16949)
ASPICE Assessment → Gap Analysis → Improvement Plan → Process Evidence        (ASPICE PAM)
```

Every box above has a builder skill AND a matching reviewer skill.

## Skills

### Foundation (3 builder/reviewer pairs)
| Skill | Standard |
|-------|----------|
| `item-definition-builder` + reviewer | ISO 26262-3 §5 |
| `safety-plan-builder` + reviewer | ISO 26262-2 §6 |
| `dia-builder` + reviewer | ISO 26262-8 §5 |

### ISO 26262 Concept Phase (3 pairs)
| Skill | What it produces |
|-------|------------------|
| `hara-builder` + reviewer | Hazard Analysis with 14 malfunction guide words and Cartesian (function × malfunction × environment) |
| `fsc-builder` + reviewer | FTA per safety goal with visual fault tree rendering, FSR derivation, ASIL decomposition |
| `tsc-builder` + reviewer | HW-TSR/SW-TSR with safety mechanisms, HSI scaffold, system FMEA, DFA, draw.io architecture diagram |

### ISO 26262 HW Lane (4 pairs, Part 5)
| Skill | What it produces |
|-------|------------------|
| `hw-safety-reqs-builder` + reviewer | HW Safety Requirements derived from HW-TSRs |
| `hw-architecture-builder` + reviewer | Refined HW architecture (PCB / IC / package) with safety mechanism allocation |
| `hsi-builder` + reviewer | Contract-grade HW-SW Interface with C header generation |
| `fmeda-builder` + reviewer | Formula-driven SPFM/LFM/PMHF with auto-verification against per-ASIL targets |

### ISO 26262 SW Lane (4 pairs, Part 6)
| Skill | What it produces |
|-------|------------------|
| `sw-arch-builder` + reviewer | SW architecture with RTOS, scheduling, MPU partitioning, MISRA targets |
| `sw-sr-builder` + reviewer | SW Safety Requirements with verification methods per ASIL |
| `sw-fmea-builder` + reviewer | Software FMEA with qualitative residual risk per ASIL threshold |
| `sw-hsis-builder` + reviewer | SW-side HSI with handler functions and C header |

### ISO 26262 Capstone
| Skill | What it produces |
|-------|------------------|
| `safety-case-builder` + reviewer | GSN-style argument pulling evidence from all upstream artifacts |

### ISO/SAE 21434 Cybersecurity (6 pairs, UN R155 mandated)
| Skill | What it produces |
|-------|------------------|
| `tara-builder` + reviewer | Threat Analysis with STRIDE coverage and Risk Value lookup |
| `cs-goals-builder` + reviewer | Cybersecurity Goals with CAL assignment |
| `cs-concept-builder` + reviewer | CSR derivation per cybersecurity property |
| `cs-architecture-builder` + reviewer | Cryptographic inventory, key management, secure boot chain |
| `incident-response-plan-builder` + reviewer | PSIRT plan with UN R155 / ENISA / NHTSA notification deadlines |
| `secure-coding-guidelines-builder` + reviewer | CERT C + MISRA Amendment 1 + AUTOSAR C++14 secure subset |

### ISO 21448 SOTIF (3 pairs, ADAS / AV)
| Skill | What it produces |
|-------|------------------|
| `sotif-analysis-builder` + reviewer | ODD, performance limitations, scenario quadrant classification |
| `triggering-conditions-builder` + reviewer | Environmental / sensor / road / road-user / system / driver triggering conditions |
| `sotif-validation-strategy-builder` + reviewer | Real-world / simulation / proving ground / field monitoring campaign |

### AIAG-VDA Quality (5 pairs, IATF 16949)
| Skill | What it produces |
|-------|------------------|
| `apqp-plan-builder` + reviewer | 5-phase product quality plan with gate reviews |
| `dfmea-builder` + reviewer | Design FMEA per AIAG-VDA 2019 (Action Priority replaces RPN) |
| `pfmea-builder` + reviewer | Process FMEA per AIAG-VDA 2019 |
| `control-plan-builder` + reviewer | Production Control Plan with SPC / 100% / mistake-proofing / audit |
| `ppap-package-builder` + reviewer | 18-element PPAP submission with PSW |

### Automotive SPICE (4 pairs)
| Skill | What it produces |
|-------|------------------|
| `aspice-assessment-builder` + reviewer | Per-process Capability Level scoring per PAM 3.1 |
| `aspice-gap-analysis-builder` + reviewer | Gaps to target CL with severity classification |
| `aspice-improvement-plan-builder` + reviewer | Improvement roadmap with KPIs |
| `aspice-process-evidence-builder` + reviewer | Per-process evidence package |

### Continuous Improvement (5 pairs)
| Skill | What it produces |
|-------|------------------|
| `8d-problem-solving-builder` + reviewer | 8D method (D1-D8) for warranty / customer complaint |
| `5-why-builder` + reviewer | 5-Why root cause analysis |
| `fishbone-builder` + reviewer | Ishikawa (6Ms) cause-and-effect with auto-Mermaid diagram |
| `msa-gage-rr-builder` + reviewer | AIAG MSA 4th edition with Anova-based Gauge R&R |
| `spc-chart-builder` + reviewer | SPC with Cpk capability and Western Electric / Nelson rules |

### Communication protocols (8 pairs)
| Skill | What it produces |
|-------|------------------|
| `dbc-builder` + reviewer | Vector DBC (CAN database) with messages, signals, ECUs, multiplexed groups |
| `ldf-builder` + reviewer | LIN Description File per ISO 17987 / LIN 2.2A |
| `arxml-system-builder` + reviewer | AUTOSAR system extract (ECUs, system mappings, comm stack, port interfaces) |
| `communication-matrix-builder` + reviewer | K-Matrix across CAN / CAN FD / LIN / FlexRay / Automotive Ethernet |
| `bus-load-analysis-builder` + reviewer | Bandwidth utilization with worst-case and fault-injection scenarios |
| `gateway-routing-builder` + reviewer | Cross-bus signal / PDU / frame routing with latency and security rules |
| `flexray-config-builder` + reviewer | FlexRay cluster config per ISO 17458 (static + dynamic segments) |
| `automotive-ethernet-builder` + reviewer | 100BASE-T1 / 1000BASE-T1 with SOME/IP, AVB, TSN, VLAN |

### Diagnostics (5 pairs)
| Skill | What it produces |
|-------|------------------|
| `uds-services-builder` + reviewer | UDS service catalog per ISO 14229-1 with DIDs, RIDs, security access |
| `dtc-catalog-builder` + reviewer | DTC catalog per ISO 14229 / SAE J2012 with snapshots, extended data |
| `cdd-builder` + reviewer | Vector CANdela diagnostic database authoring |
| `odx-builder` + reviewer | ODX per ISO 22901-1 with base / derived variants and PDU library |
| `dem-config-builder` + reviewer | AUTOSAR DEM config with debounce, healing, NvM, OBD freeze frame |

### AUTOSAR (5 pairs)
| Skill | What it produces |
|-------|------------------|
| `autosar-swc-builder` + reviewer | AUTOSAR Classic SWC with ports, runnables, events, exclusive areas |
| `autosar-composition-builder` + reviewer | Composition SWC with assembly + delegation connectors |
| `autosar-bsw-config-builder` + reviewer | BSW config: MCAL, ECU Abstraction, Service Layer, Complex Drivers |
| `autosar-rte-mapping-builder` + reviewer | RTE mapping: signal-to-port, runnable-to-task, exclusive area binding |
| `autosar-adaptive-app-builder` + reviewer | AUTOSAR Adaptive app + service + machine manifests, ara::com / ara::exec |

### Calibration (3 pairs)
| Skill | What it produces |
|-------|------------------|
| `a2l-builder` + reviewer | A2L per ASAM MCD-2 MC v1.7 (characteristics, measurements, conversions) |
| `dcm-builder` + reviewer | ETAS DCM dataset for cross-tool calibration value exchange |
| `calibration-data-exchange-builder` + reviewer | OEM ↔ Tier-1 calibration ownership and exchange agreement |

### MBSE (3 pairs)
| Skill | What it produces |
|-------|------------------|
| `mbse-model-architecture-builder` + reviewer | ARCADIA-style operational / system / logical / physical layers |
| `mbse-requirements-allocation-builder` + reviewer | Requirements decomposition with satisfy / derive / verify relationships |
| `mbse-system-context-builder` + reviewer | System boundary, actors, use cases, stakeholder needs |

### SysML (4 pairs)
| Skill | What it produces |
|-------|------------------|
| `sysml-block-diagram-builder` + reviewer | BDD / IBD with blocks, parts, ports, connectors, item flows |
| `sysml-requirement-diagram-builder` + reviewer | Requirement blocks with derive / satisfy / verify / refine relationships |
| `sysml-activity-diagram-builder` + reviewer | Activities, control / object flows, decision / fork / join, swim-lanes |
| `sysml-state-machine-builder` + reviewer | States, transitions with guards / effects, regions, history pseudo-states |

### V&V (5 pairs)
| Skill | What it produces |
|-------|------------------|
| `verification-plan-builder` + reviewer | Verification Plan per ISO 26262-8 §9 / IEEE 1012 |
| `validation-plan-builder` + reviewer | Item Validation Plan per ISO 26262-4 §9 |
| `test-case-catalog-builder` + reviewer | Test cases with preconditions, inputs, expected outputs, traceability |
| `traceability-matrix-builder` + reviewer | Bidirectional RTM with coverage analysis and orphan identification |
| `vv-execution-report-builder` + reviewer | V&V campaign report with defects, coverage, residual risk acceptance |

### Chain-integrated program management (5 pairs)
| Skill | What it produces |
|-------|------------------|
| `safety-program-risk-register-builder` + reviewer | Programmatic risks affecting safety case delivery |
| `safety-gate-review-builder` + reviewer | ISO 26262 phase-gate reviews with Go / Conditional / No-Go |
| `wp-status-rollup-builder` + reviewer | Work product completion roll-up across the safety lifecycle |
| `change-impact-analysis-builder` + reviewer | Change impact across HARA / FSC / TSC / HW-SW reqs / tests |
| `lessons-learned-builder` + reviewer | Lessons learned register per ISO 26262-2 §5 / IATF 16949 |

## Standards covered

ISO 26262:2018 · ISO/SAE 21434:2021 · ISO 21448:2022 · IATF 16949:2016 · AIAG APQP 2nd edition · AIAG-VDA FMEA Handbook 2019 · AIAG PPAP 4th edition · AIAG MSA 4th edition · AIAG SPC 2nd edition · AIAG Control Plan · ASPICE PAM 3.1 / 4.0 · UN R155 · UN R156 · CERT C Secure Coding · MISRA C:2012 + Amendment 1 · AUTOSAR C++14 · AUTOSAR Classic R22-11 · AUTOSAR Adaptive R22-11 · ISO 14229-1 (UDS) · SAE J2012 (DTCs) · ISO 22901-1 (ODX) · ISO 11898 (CAN) · ISO 17987 (LIN) · ISO 17458 (FlexRay) · IEEE 100BASE-T1 / 1000BASE-T1 · ASAM MCD-2 MC (A2L) · OMG SysML 1.6 / 2.0 · INCOSE Systems Engineering Handbook · IEEE 1012 (V&V)

## Why

Every automotive program ships an avalanche of structured deliverables. Most teams maintain them in xlsx, painstakingly hand-edited, with low audit-trail consistency and high re-keying overhead between phases. This suite turns each phase into a builder + reviewer pair where:

- The **builder** produces the structured xlsx output from a JSON input plus the upstream artifact
- The **reviewer** runs a confirmation-measures checklist on the builder's output and produces a visual dashboard with KPIs, charts, and findings

The chain compounds because phases hand off via stable xlsx contracts. A change upstream (HARA) automatically propagates to every downstream skill that consumes it (FSC → TSC → FMEDA → Safety Case).

## Common patterns across the suite

- All builder outputs use a consistent NAVY/Calibri styling palette with ASIL/CAL color coding
- All reviewer outputs include visual dashboards (KPI tiles, pie chart, compliance bar, stacked rating breakdown by section, findings table)
- Many reviewers auto-verify quantitative thresholds:
  - **FMEDA**: SPFM / LFM / PMHF vs ASIL target
  - **PPAP**: Cpk vs threshold (1.33 standard, 1.67 special characteristics)
  - **SW FMEA**: residual risk vs ASIL threshold
  - **MSA**: Gauge R&R % vs AIAG threshold
  - **SPC**: Cpk vs target
- Reviewers never modify source artifacts — gaps land in Recommended Actions
- Every builder is paired with a reviewer for confirmation-measures compliance

## Repo structure

```
automotive-skills-suite/
├── skills/                      # 152 .skill bundles, install individually
├── README.md
└── LICENSE
```

## Installation

Each `.skill` file in `skills/` is independently installable. Skills declare triggering descriptions in their frontmatter, so the agent picks them up based on user phrasing (e.g., "build me a HARA" triggers `hara-builder`).

## License

MIT — see LICENSE file.

---

<div align="center">

Built with Claude. Designed for automotive safety, cybersecurity, and quality engineers shipping real programs.

</div>
