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

This path must be configured before using `Invoke-AtomicTest`.

---

### 4.1 Configure Path for Current Session (Temporary)

Set the environment variable:

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

⚠️ This configuration applies **only to the current PowerShell session**.
Closing PowerShell will reset it.

---

### 4.2 Configure Persistent Path (Recommended)

To ensure the correct Atomic path is automatically used in every session, configure your PowerShell profile.

#### Step 1: Identify Profile Location

```powershell
$PROFILE
```

If the file does not exist:

```powershell
New-Item -ItemType File -Path $PROFILE -Force
```

---

#### Step 2: Edit Profile

```powershell
notepad $PROFILE
```

Add the following lines:

```powershell
$env:ATOMIC_RED_TEAM_PATH = "C:\Tools\atomic-red-team"

$PSDefaultParameterValues["Invoke-AtomicTest:PathToAtomicsFolder"] = `
    "$env:ATOMIC_RED_TEAM_PATH\atomics"
```

Save and close the file.

---

#### Step 3: Restart PowerShell

Close all PowerShell windows and reopen.

Validate configuration:

```powershell
Invoke-AtomicTest T1003 -ShowDetailsBrief
```

If the command runs without referencing:

```
C:\AtomicRedTeam\atomics
```

then configuration is correct.

---

### Why This Configuration Is Required

`Invoke-AtomicTest` contains a hardcoded default path:

```
C:\AtomicRedTeam\atomics
```

Using `$PSDefaultParameterValues` ensures:

* The correct lab path is automatically supplied
* Manual `-PathToAtomicsFolder` parameters are not required
* The environment remains consistent across reboots
* The lab setup is fully reproducible

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
* `Invoke-AtomicTest T1003 -ShowDetailsBrief` works without specifying a path

No tests should be executed as part of this guide.

---
