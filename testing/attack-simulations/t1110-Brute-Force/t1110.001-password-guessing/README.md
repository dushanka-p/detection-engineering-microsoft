# T1110.001 – Password Guessing (Guide)

This guide documents how to reproduce authentication-failure telemetry
used to engineer and validate detections in this repository.

---

## Preconditions

- Microsoft Entra ID tenant
- Test user account (non-privileged)
- Microsoft Sentinel enabled
- Access to SignInLogs table

---

## Reproduction Steps

1. Open a private browser session
2. Navigate to https://login.microsoftonline.com
3. Enter the test user’s UPN
4. Submit an incorrect password
5. Repeat 5–10 times within 2–5 minutes
6. Stop before account lockout threshold

---

## Validation

Confirm telemetry is present:

```kql
SigninLogs
| where TimeGenerated >= ago(30m)
| where UserPrincipalName  =~ "UPN-HERE"
| where ResultType != 0
| project TimeGenerated, UserPrincipalName, ResultType, IPAddress, ClientAppUsed, AppDisplayName
| order by TimeGenerated desc
```
Expected outcomes:
- Multiple failed sign-ins for one user
- Consistent IP and ClientAppUsed
- ResultType values such as 50126
