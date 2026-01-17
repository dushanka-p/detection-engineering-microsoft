# detection-engineering-microsoft
This repo shows how I design and validate detections using Microsoft Sentinel and Defender telemetry.
```
detection-engineering/
├── README.md

├── meta/
│   ├── data-sources.md
│   ├── naming-conventions.md
│   └── maturity-model.md

├── testing/
│   └── attack-simulations/
│       ├── _template/
│       │   ├── attack.md
│       │   ├── telemetry.md
│       │   └── kql.md
│       ├── T1059_Command_and_Scripting/
│       ├── T1110_Brute_Force/
│       └── T1078_Valid_Accounts/

├── hunting/
│   └── _template.kql

├── detections/
│   ├── endpoint/
│   │   ├── experimental/
│   │   └── production/
│   ├── identity/
│   ├── email/
│   ├── cloud/
│   └── network/

├── response/
│   └── _template.md



```
