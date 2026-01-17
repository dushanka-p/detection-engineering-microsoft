# Atomic Red Team – Installation & Lab Readiness Guide

This guide shows how to **install and prepare Atomic Red Team** for use in the
`detection-engineering-microsoft` lab.

**Purpose**

* Generate controlled attacker telemetry
* Validate Microsoft Sentinel & Defender visibility
* Support repeatable detection engineering testing

⚠️ **Lab use only. Never run in production.**

---

## 1. Scope & Assumptions

**Environment**

* Windows test VM (non-production)
* Microsoft Defender for Endpoint onboarded
* Microsoft Sentinel connected and ingesting logs
* Local administrator access

**Out of scope**

* Red team operations
* Live malware deployment
* Production execution

---

## 2. Prerequisites

### 2.1 PowerShell

* PowerShell 5.1 or later

Allow script execution (current user only):

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

### 2.2 Git

Required to clone Atomic Red Team.

Verify Git is installed:

```powershell
git --version
```

If Git is not installed, install it before continuing.

---

## 3. Installation Steps

### Step 1: Clone Atomic Red Team

```powershell
cd C:\Tools
git clone https://github.com/redcanaryco/atomic-red-team.git
```

Expected path:

```
C:\Tools\atomic-red-team
```

---

### Step 2: Import the Atomic PowerShell Module

```powershell
cd C:\Tools\atomic-red-team
Import-Module .\atomics\Invoke-AtomicRedTeam.psd1
```

Verify installation:

```powershell
Get-Command Invoke-AtomicTest
```

If the command exists, Atomic Red Team is ready.

---

## 4. Dependency Handling (Important)

Atomic tests may require dependencies such as:

* PowerShell
* cmd.exe
* bitsadmin
* certutil
* curl / WebClient

### Rule

👉 **Install dependencies per test, not globally**

Example:

```powershell
Invoke-AtomicTest T1059 -GetPrereqs
```

Do **not** preinstall everything “just in case”.

---

## 5. Execution Model (How to Run Tests)

### Rule: One MITRE Technique at a Time

Atomic tests are executed **by MITRE technique**, matching this repo structure:

```
testing/attack-simulations/
└── T1059_Command_and_Scripting/
```

Example execution:

```powershell
Invoke-AtomicTest T1059 -TestNumbers 1
```

---

## 6. Documentation Rule (Mandatory)

Every Atomic execution **must** be documented.

Record:

* Technique ID
* Test number
* Timestamp
* Hostname
* Expected telemetry

This maps directly to:

* `attack.md`
* `telemetry.md`

If it isn’t documented, it didn’t happen.

---

## 7. Defender & Sentinel Validation

After execution, validate telemetry in Sentinel.

### Endpoint Tables

* `DeviceProcessEvents`
* `DeviceFileEvents`
* `DeviceNetworkEvents`

### Identity Tables (if applicable)

* `SigninLogs`
* `AuditLogs`

A blocked test still counts as **successful telemetry generation**.

---

## 8. Safety Rules (Non-Negotiable)

* ❌ Never run Atomics on production systems
* ❌ Never auto-run `-Cleanup` without review
* ❌ Never modify Atomic tests directly

All adjustments must be documented **in this repo**, not upstream.

---

## 9. Common Notes

* Some tests are noisy by design
* Some tests will be blocked by Defender
* Blocked ≠ failed (telemetry still matters)
* Results may vary based on Defender configuration

---

## 10. Detection Engineering Workflow Mapping

| Phase          | Artifact             |
| -------------- | -------------------- |
| Execute Atomic | `attack.md`          |
| Observe Logs   | `telemetry.md`       |
| Explore Logic  | `hunting/*.kql`      |
| Create Alert   | `detections/*/*.kql` |
| SOC Action     | `response/*.md`      |

Atomic Red Team is **a telemetry generator**, not the goal.

---

## 11. References

* Atomic Red Team (Red Canary)
* MITRE ATT&CK Framework
* Microsoft Defender for Endpoint documentation

---
