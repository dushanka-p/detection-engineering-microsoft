# Running Atomic Red Team Tests (Lab Execution Guide)

This guide documents **how to safely execute Atomic Red Team tests** in the
`detection-engineering-microsoft` lab environment.

⚠️ **Lab use only. Never run on production systems.**

---

## Scope

This guide covers:

* How to list available Atomic tests
* How to run individual Atomic tests
* How to handle prerequisites and cleanup
* What to record after execution

Out of scope:

* Installation of Atomic Red Team
* Detection logic or KQL development
* Sentinel or Defender analysis
* Detection promotion or tuning

---

## Preconditions (Must Be True)

Before running any tests:

* Atomic Red Team is installed
* `Invoke-AtomicTest` is available
* `$env:ATOMIC_RED_TEAM_PATH` is set correctly
* You are on a **non-production** Windows VM

If any of the above are false, stop and fix installation first.

---

## 1. Rule Zero (Read This Once)

👉 **Run one test at a time.**
👉 **Never bulk-run all tests for a technique.**
👉 **Assume Defender may block execution.**

Blocked activity is still **successful telemetry generation**.

---

## 2. List Available Tests for a Technique

Before executing anything, review what exists.

Example (T1059):

```powershell
Invoke-AtomicTest T1059 -ShowDetailsBrief
```

This shows:

* Test numbers
* Test descriptions
* What each test attempts to do

Do not execute blindly.

---

## 3. Install Prerequisites (If Required)

Some tests require additional tools or configuration.

### Rule

👉 **Install prerequisites per test, not globally**

Example:

```powershell
Invoke-AtomicTest T1059 -TestNumbers 1 -GetPrereqs
```

If prerequisites fail to install:

* Document the failure
* Do not workaround silently

---

## 4. Execute a Single Atomic Test

Run **one test number only**.

Example:

```powershell
Invoke-AtomicTest T1059 -TestNumbers 1
```

Expected outcomes:

* Test executes successfully **or**
* Test is blocked by Defender

Both outcomes are valid.

---

## 5. Cleanup (Manual Review Required)

Some tests provide cleanup steps.

⚠️ **Never auto-run cleanup without review**

If cleanup is required:

```powershell
Invoke-AtomicTest T1059 -TestNumbers 1 -Cleanup
```

Only run cleanup if:

* You understand what it removes
* It will not break your lab state

---

## 6. Documentation Rule (Mandatory)

Every Atomic test execution **must be documented**.

Record at minimum:

* MITRE Technique ID
* Test number
* Date and time
* Hostname
* Execution outcome (allowed / blocked)
* Expected telemetry

This maps directly to:

* `attack.md`
* `telemetry.md`

If it isn’t written down, it didn’t happen.

---

## 7. Common Outcomes (Expected)

* Defender blocks execution → **normal**
* Partial execution → **normal**
* No visible alert → **normal**
* Telemetry without alert → **desired**

Atomic tests validate **telemetry**, not alerting.

---

## 8. Hard Safety Rules

* ❌ Never run all tests for a technique
* ❌ Never disable Defender protections to “make it work”
* ❌ Never modify Atomic test code
* ❌ Never run tests outside the lab

Atomic Red Team is a **controlled adversary simulation**, not a red team exercise.

---

## Completion Criteria

A test run is considered complete when:

* The test was executed or blocked
* The outcome was recorded
* No unreviewed changes remain on the system

Detection work starts **after** this point.

---

## Next Step

After running a test:

1. Populate `attack.md`
2. Populate `telemetry.md`
3. Begin exploratory hunting (`hunting/*.kql`)
4. Decide if a detection is warranted

Execution is only the **first step**.

---
