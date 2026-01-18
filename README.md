# detection-engineering-microsoft
This repo shows how I design and validate detections using Microsoft Sentinel and Defender telemetry.

## Detection → Alert → SOC Investigation Flow

This repository follows a production-aligned workflow where detections are
engineered, validated, and then consumed by SOC operations.

📄 Detailed flow:
docs/detection-to-soc-flow.md

```
detection-engineering-microsoft/
├── README.md

├── meta/
│   ├── data-sources.md
│   ├── naming-conventions.md
│   └── maturity-model.md

├── testing/
│   └── attack-simulations/
│       ├── atomic-red-team-setup.md        # install-only guide
│       ├── running-atomic-tests.md         # execution-only guide
│       ├── _template/
│       │   ├── attack.md
│       │   ├── telemetry.md
│       │   └── kql.md
│       ├── T1059_Command_and_Scripting/
│       ├── T1110_Brute_Force/
│       └── T1078_Valid_Accounts/

├── detections/
│   ├── _template/
│   │   └── detection.md                   # canonical detection template
│   ├── endpoint/
│   │   └── DE-ENDP-001-example-detection/
│   │       ├── rule.kql
│   │       └── detection.md
│   ├── identity/
│   │   └── DE-IDENT-001-example-detection/
│   │       ├── rule.kql
│   │       └── detection.md
│   ├── email/
│   ├── cloud/
│   └── network/

├── response/
│   └── _template.md

```
