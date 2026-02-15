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

# 1. Prerequisites

## PowerShell

* PowerShell 5.1 or later

Allow script execution for the current user:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

## Git

Git is required to download Atomic Red Team.

Verify Git is available:

```powershell
git --version
```

---

# 2. Install Atomic Red Team (Attack Definitions)

## Step 1: Clone the Repository

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

# 3. Install the PowerShell Execution Module

Atomic tests are executed using the **Invoke-AtomicRedTeam** PowerShell module, which is installed separately from the PowerShell Gallery.

## Step 2: Install the Module (Administrator Required)

Open PowerShell **as Administrator** and run:

```powershell
Install-Module Invoke-AtomicRedTeam -Scope AllUsers -Force
```

If prompted:

* NuGet provider → Accept
* Untrusted repository → Accept

---

## Step 3: Import and Verify the Module

```powershell
Import-Module Invoke-AtomicRedTeam
```

Verify installation:

```powershell
Get-Command Invoke-AtomicTest
```

If `Invoke-AtomicTest` is returned, the module is installed correctly.

---

# 4. Configure Atomic Path (Machine-Persistent – Recommended)

By default, the module expects Atomic tests at:

```
C:\AtomicRedTeam\atomics
```

In this lab, Atomics are stored at:

```
C:\Tools\atomic-red-team\atomics
```

This path must be configured.

---

## Step 4.1 – Configure Machine-Level Environment Variable

Open PowerShell **as Administrator** (standard PowerShell, not ISE required) and run:

```powershell
[Environment]::SetEnvironmentVariable(
  "ATOMIC_RED_TEAM_PATH",
  "C:\tools\atomic-red-team",
  "Machine"
)
```

This sets the variable at the **machine level**, meaning:

* It survives reboots
* It works in PowerShell, PowerShell ISE, and other shells
* It does not rely on PowerShell profiles

---

## Step 4.2 – Restart PowerShell

Close all PowerShell windows completely.

Reopen PowerShell and verify:

```powershell
echo $env:ATOMIC_RED_TEAM_PATH
```

Expected output:

```
C:\tools\atomic-red-team
```

---

## Step 4.3 – Validate Atomics Path

```powershell
Test-Path "$env:ATOMIC_RED_TEAM_PATH\atomics"
```

Expected output:

```
True
```

---

# Why This Method Is Recommended

Using a machine-level environment variable:

* Avoids profile-related issues (PowerShell vs ISE differences)
* Avoids session-only configuration problems
* Keeps lab configuration consistent and reproducible
* Reflects more production-aligned configuration practice

This method is more stable than profile-based configuration.

---

# 5. Cautions & Safety Notes

* ❌ Never install or run Atomic Red Team on production systems
* ❌ Do not modify Atomic tests directly
* ❌ Do not assume tests are safe — review before execution
* ❌ Do not install dependencies globally unless required by a specific test

Atomic Red Team is a **telemetry generation framework**, not a payload delivery tool.

---
