# investigation – t1059.001 test-03

---

## detection summary

incident classification:
- compromised account conducting hands-on-keyboard attack

severity:
- high

automatic response:
- contain user
- actiontype: containeduserlogonblocked

---

## telemetry analysis

### deviceprocessevents

- powershell.exe observed
- suspicious execution attempt
- command associated with in-memory download behaviour

execution appears to have been blocked prior to full script retrieval.

---

### devicenetworkevents

observed outbound connections to:

- powershellgallery.com
- cdn.oneget.org
- go.microsoft.com

no confirmed successful connection to raw.githubusercontent.com for sharphound payload.

indicates pre-execution or early-stage block.

---

### deviceevents

actiontype observed:

- containeduserlogonblocked

defender blocked network logon attempts for the user account.

---

## containment behaviour analysis

defender automatic attack disruption:

- restricted user account
- blocked ntlm network authentication
- prevented bastion / rdp login
- did not isolate the device

distinction:

- user containment != device isolation

---

## root cause of access issue

bastion relies on network logon via ntlm.

containment blocked:

- logontype = network
- authentication attempts from remote session

therefore login failed despite device being active.

---

## key technical findings

- defender blocked execution before sharphound completed
- no evidence of successful data collection
- containment was precautionary based on behavioural confidence
- recovery required manual reversal via action center

---

## detection engineering insight

this test demonstrates:

- behaviour-based detection triggering before payload execution
- automated containment impact on administrative workflow
- importance of separating attack user and admin user in lab design

future lab improvement:

- create break-glass admin account
- snapshot before high-risk atomic tests
