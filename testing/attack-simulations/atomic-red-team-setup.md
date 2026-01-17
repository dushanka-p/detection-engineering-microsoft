# Atomic Red Team – Installation Guide (Lab Only)

This guide documents the **minimum steps required to install Atomic Red Team** for use in the
`detection-engineering-microsoft` lab environment.

⚠️ **Lab use only. Never run on production systems.**

---

## Scope

This guide covers **installation and setup only**.

Out of scope:

* How to run Atomic tests
* Detection validation
* Sentinel or Defender analysis
* Detection engineering workflow

Those steps are documented elsewhere in this repository.

---

## Assumptions

* Windows test VM (non-production)
* Local administrator access
* Microsoft Defender for Endpoint already onboarded
* Internet access for GitHub and PowerShell Gallery

---

## 1. Prerequisites

### PowerShell

* PowerShell 5.1 or later

Allow script execution for the current user:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

### Git

Git is required to download Atomic Red Team.

Verify Git is available:

```powershell
git --version
```

---

## 2. Install Atomic Red Team (Attack Definitions)

### Step 1: Clone the Repository

```powershell
cd C:\Tools
git clone https://github.com/redcanaryco/atomic-red-team.git
```

Expected path:

```
C:\Tools\atomic-red-team
```

This repository contains:

* Atomic test definitions
* MITRE ATT&CK mappings
* Supporting documentation

⚠️ This repository **does NOT include** the PowerShell execution module.

---

## 3. Install the PowerShell Execution Module

Atomic tests are executed using the **Invoke-AtomicRedTeam** PowerShell module, which is installed **separately** from the PowerShell Gallery.

### Step 2: Install the Module (Administrator Required)

Open PowerShell **as Administrator**, then run:

```powershell
Install-Module Invoke-AtomicRedTeam -Scope AllUsers -Force
```

If prompted:

* NuGet provider → **Accept**
* Untrusted repository → **Accept**

---

### Step 3: Import and Verify the Module

```powershell
Import-Module Invoke-AtomicRedTeam
```

Verify installation:

```powershell
Get-Command Invoke-AtomicTest
```

If `Invoke-AtomicTest` is returned, the module is installed correctly.

---

## 4. Configure Atomic Path (Required)

By default, the module expects Atomic tests at:

```
C:\AtomicRedTeam\atomics
```

In this lab, Atomics are stored at:

```
C:\Tools\atomic-red-team\atomics
```

Set the correct path for the current session:

```powershell
$env:ATOMIC_RED_TEAM_PATH = "C:\Tools\atomic-red-team"
```

Verify:

```powershell
Test-Path "$env:ATOMIC_RED_TEAM_PATH\atomics"
```

Expected output:

```
True
```

> Optional: persist this setting in your PowerShell profile if required.

---

## 5. Cautions & Safety Notes

* ❌ Never install or run Atomic Red Team on production systems
* ❌ Do not modify Atomic tests directly
* ❌ Do not assume tests are safe — review before execution
* ❌ Do not install dependencies globally unless required by a specific test

Atomic Red Team is a **telemetry generator**, not a payload delivery tool.

---

## Completion Criteria

Installation is complete when:

* Atomic Red Team repo exists under `C:\Tools\atomic-red-team`
* `Invoke-AtomicTest` is available in PowerShell
* `$env:ATOMIC_RED_TEAM_PATH\atomics` resolves correctly

No tests should be executed as part of this guide.

---
