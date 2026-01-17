# Detection Engineering – Naming & Metadata Conventions

This document defines the **mandatory standards** for naming, metadata, and structure
across the `detection-engineering-microsoft` repository.

These conventions exist to ensure:
- Consistency across detections, hunts, simulations, and response artifacts
- Clear ownership and lifecycle tracking
- Accurate MITRE ATT&CK coverage without filesystem sprawl
- Readability for SOC analysts and detection engineers

---

## 1. Core Principles

1. **Folders represent ownership, not full coverage**
   - MITRE techniques are many-to-many
   - Filesystems are one-to-one
   - Coverage truth lives in metadata, not folder names

2. **Every artifact must be self-describing**
   - If a file is opened in isolation, its purpose must be obvious
   - Metadata headers are mandatory

3. **Primary MITRE technique drives placement**
   - Secondary techniques are always declared in metadata
   - No duplication of files across MITRE folders

---

## 2. Repository Naming

### Repository Name
detection-engineering-microsoft


Scope is explicitly Microsoft-centric (Sentinel, Defender, Entra, M365).

---

## 3. Folder Naming Standards

### MITRE Technique Folders
Format:
Txxxx_Technique_Name_With_Underscores


Examples:
T1059_Command_and_Scripting
T1110_Brute_Force
T1078_Valid_Accounts


Rules:
- Use **primary technique only**
- Technique name matches MITRE ATT&CK naming (no custom wording)
- Case-sensitive consistency is required

---

## 4. Detection Lifecycle Structure

### Testing
testing/attack-simulations/

Purpose:
- Validate telemetry exists
- Reproduce attacker behavior
- Map activity to MITRE techniques

---

### Hunting
hunting/

Purpose:
- Hypothesis-driven exploration
- Not production alerts
- Expected to be noisy

---

### Detections
detections/<domain>/
├── experimental/
└── production/


Rules:
- Experimental = tuning, validation, learning
- Production = SOC-facing alerts only
- Promotion requires documented tuning and confidence increase

---

### Response
response/

Purpose:
- Human actions
- SOC playbooks
- No detection logic

---

## 5. Mandatory Metadata Header

Every artifact **must** begin with the standard metadata header:
- Markdown files (`.md`)
- KQL files (`.kql`)

If metadata is missing, the artifact is considered **non-compliant**.

---

## 6. Detection ID Convention

Format:
DE-MICROSOFT-<TYPE>-<NNN>


Types:
- ATTACK
- HUNT
- DETECT
- RESP

Examples:
- `DE-MICROSOFT-ATTACK-001`
- `DE-MICROSOFT-HUNT-012`
- `DE-MICROSOFT-DETECT-027`

IDs are immutable once assigned.

---

## 7. MITRE ATT&CK Rules

### Required
- One **Primary Technique**
- Zero or more **Secondary Techniques**
- One or more **Tactics**

### Placement Rule
If a detection maps to multiple techniques:
- Folder = **primary technique**
- Metadata = **full coverage**

MITRE is a **tagging system**, not a filesystem.

---

## 8. Promotion Rules (Experimental → Production)

A detection may be promoted when:
- Telemetry is validated
- Known false positives are documented
- Tuning guidance exists
- Confidence is Medium or High

Promotion must update:
- Status field
- Revision history
- Confidence level

---

## 9. Non-Goals (Explicitly Out of Scope)

- CVE tracking
- Tool version pinning
- Vendor-specific naming schemes
- Automated enforcement (for now)

This repository prioritizes **clarity and signal over completeness**.

---

**If it’s not documented in metadata, it does not exist.**
