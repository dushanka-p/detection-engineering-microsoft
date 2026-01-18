# Attack Simulation – Failed Sign-in Burst (T1110)

## Objective
Generate multiple failed Entra ID sign-in attempts for a single user
to observe authentication telemetry and error patterns.

## Technique
MITRE ATT&CK: T1110 – Brute Force (Password Guessing)

## Execution Plan
- Use a test user account
- Attempt multiple invalid passwords
- Perform attempts within a short time window
- No account lockout enforcement bypass

## Expected Telemetry
- Azure AD / Entra ID SignInLogs
- ResultType indicating authentication failure
- Repeated failures from same IP or client

## Notes
This simulation is performed in a controlled lab environment only.

