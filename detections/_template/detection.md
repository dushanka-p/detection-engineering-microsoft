# Detection: <Short, human-readable name>

## Detection ID
- <DE-ENDP-001 | DE-IDENT-001 | DE-EMAIL-001 | DE-CLOUD-001 | DE-NET-001>

## Status
- Experimental | Production | Deprecated

## Summary
- One sentence describing what this detects and why it matters.

## Data Source
- Product: <Microsoft Defender for Endpoint | Microsoft Sentinel | Entra ID | M365 Defender | Defender for Office 365>
- Tables / Logs:
  - <e.g., DeviceProcessEvents>
  - <e.g., SigninLogs>
- Required fields:
  - <list the key fields you rely on>

## MITRE ATT&CK Mapping
- Technique(s):
  - <e.g., T1059 Command and Scripting Interpreter>
- Sub-technique(s) (if applicable):
  - <e.g., T1059.001 PowerShell>

## Detection Logic (High Level)
- What pattern you’re looking for (plain English).
- Any thresholds/time windows.
- Key exclusions (if any).

## Query
- File: `rule.kql`
- Notes:
  - <anything important about how the query works>

## Alert Details
- Recommended severity: Low | Medium | High
- Recommended tactics: <Execution / Persistence / Credential Access ...>
- Entities expected:
  - Account
  - Host
  - IP
  - Process
- Suppression / grouping guidance:
  - <how you’d reduce noise>

## Validation
### Test Method
- Atomic Red Team | Manual simulation | Log replay | Other

### Linked Test(s)
- `testing/attack-simulations/<Txxxx_Technique_Name>/test-001`

### Evidence
- <Add screenshots or exported results under this folder, if you use them>
- Example paths:
  - `validation/evidence/sentinel-alert.png`
  - `validation/evidence/query-results.csv`

## Tuning Notes
- Known false positives:
  - <what can trigger this legitimately>
- Common safe exclusions:
  - <admins, service accounts, known tools, trusted paths>
- Environment assumptions:
  - <what must be true for this detection to work>

## Response Playbook (Quick)
- Triage:
  - <first 3 checks>
- Investigate:
  - <next 3 deeper checks>
- Contain (if true positive):
  - <actions: isolate device, reset password, revoke sessions, etc.>
- Escalation criteria:
  - <when to escalate to L2/L3/IR>

## Versioning / Change Log
- v1.0 - <date> - Initial detection
- v1.1 - <date> - <tuning change>
