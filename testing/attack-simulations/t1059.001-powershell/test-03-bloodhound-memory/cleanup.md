# Cleanup

## Atomic Red Team Cleanup Command

Remove-Item $env:Temp\*BloodHound.zip -Force

## What This Removes
- BloodHound zip archive dropped into %TEMP%

## Manual Verification
- Confirm no BloodHound-related files remain in:
  C:\Users\<user>\AppData\Local\Temp
- Confirm no BloodHound processes running:
  Get-Process *bloodhound*
- Confirm no scheduled tasks were created
- Confirm Defender exclusions were not modified
