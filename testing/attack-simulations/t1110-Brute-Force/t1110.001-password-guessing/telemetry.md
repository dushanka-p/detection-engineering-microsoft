## Validation

Failed sign-in events observed for `test.user@yourtenant.com`.

### Observed fields (sample event)

- **UserPrincipalName:** `test.user@yourtenant.com`
- **ResultType:** `50126` (Invalid username or password)
- **IPAddress:** `185.xxx.xx.82`
- **ClientAppUsed:** `Browser`
- **AppDisplayName:** `Azure Portal`

### Time window

- **Start:** `2026-01-18T12:37:33Z`
- **End:** `2026-01-18T12:42:33Z`  

### Validation query

```kql
SigninLogs
| where TimeGenerated >= ago(30m)
| where UserPrincipalName =~ "test.user@yourtenant.com"
| where ResultType != 0
| project TimeGenerated, UserPrincipalName, ResultType, IPAddress, ClientAppUsed, AppDisplayName
| order by TimeGenerated desc

