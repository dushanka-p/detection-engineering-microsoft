# de-endp-001-powershell-inmemory-download-cradle

---

## detection id

de-endp-001

---

## primary technique

- t1059.001 – command and scripting interpreter: powershell

---

## related techniques

- t1105 – ingress tool transfer
- t1071.001 – web protocols

---

## validated against

- testing/attack-simulations/t1059.001/test-03-bloodhound-memory
- atomic test id: t1059.001-3
- test name: run bloodhound from memory using download cradle

---

## objective

detect in-memory powershell download cradle behaviour where powershell retrieves and executes remote script content using:

- iex
- new-object net.webclient
- downloadstring

focus is behavioural detection rather than specific payload.

---

## threat scenario

during atomic red team test t1059.001-3, defender blocked execution of a powershell in-memory download cradle attempting to retrieve sharphound from github.

observed behaviour:

- powershell.exe execution
- inline memory execution
- outbound https to raw.githubusercontent.com
- defender classification: trojan:powershell/sacepos.c
- automatic user containment (account SID blocked)

---

## data sources

microsoft defender for endpoint:

- deviceprocessevents
- devicenetworkevents

---

## detection logic

flag powershell execution where:

- process file name = powershell.exe
- command line contains:
  - downloadstring
  - new-object net.webclient
  - iex
- AND outbound network connection occurs shortly after

---

## severity

high

download cradles strongly correlate with:

- payload staging
- credential harvesting frameworks
- lateral movement tooling
- post-exploitation frameworks

---

## false positive considerations

possible legitimate cases:

- internal automation scripts
- devops retrieval of internal modules

noise reduction strategies:

- exclude approved internal domains
- exclude known service accounts
- baseline normal automation patterns

---

## response guidance (soc)

1. review full process command line
2. review initiating account and logon type
3. inspect devicenetworkevents for external domain
4. check if child processes spawned
5. check for persistence creation
6. isolate device if malicious
7. reset credentials if exposure suspected

---

## lab outcome

detection logic successfully identified malicious behaviour during atomic test execution.

defender blocked execution before sharphound completed.
