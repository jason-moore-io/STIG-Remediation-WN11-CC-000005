# STIG-Remediation-WN11-CC-000005

# Synopsis
  This PowerShell script ensures camera access from the lock screen is disabled.

# Notes
  Author          : Jason Moore
  LinkedIn        : linkedin.com/in/jasonmoore-infosec
  GitHub          : github.com/jasonmoore.io
  Date Created    : 2026-08-26
  Last Modified   : 2026-08-29
  Version         : 1.0
  CVEs            : N/A
  Plugin IDs      : 19506
  STIG-ID         : WN11-CC-000005
  Documentation   : https://stigaview.com/products/win11/v2r8/WN11-CC-000005/

# Tested On
  Date(s) Tested  : 2026-08-29
  Tested By       : Tenable Nessus Vulnerability Management
  Systems Tested  : Windows 11
  PowerShell Ver. : Build 26100; Revision 9168

# Usage
  Run this script in an elevated (Administrator) PowerShell session on the
  target Windows 11 workstation. It creates the NoLockScreenCamera DWORD
  under SOFTWARE\Policies\Microsoft\Windows\Personalization, sets it to
  1, then prints the confirmation output, a PASS/FAIL check, and a
  reg.exe query so you can capture evidence for the STIG checklist. No
  parameters are required.

  NOTE: If the target device does not have a camera, this control is
  Not Applicable (NA) and the script can be skipped for that system.

# To Disable
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization" 
  -Name "NoLockScreenCamera" 
  -Value 0

# Confirmation command
Get-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization" 
  -Name "NoLockScreenCamera" 
  -ErrorAction SilentlyContinue
  
 You want to see:
 NoLockScreenCamera : 0

# Remediation
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization" -Force | Out-Null
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization" 
  -Name "NoLockScreenCamera" 
  -PropertyType DWord 
  -Value 1 
  -Force | Out-Null

# Confirmation command
Get-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization" 
  -Name "NoLockScreenCamera"

 You want to see:
 NoLockScreenCamera : 1

# Cleaner pass/fail check
$Value = (Get-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Personalization" 
  -Name "NoLockScreenCamera" 
  -ErrorAction SilentlyContinue).NoLockScreenCamera

if ($Value -eq 1) {
    "PASS: WN11-CC-000005 is configured. Lock screen camera access is disabled."
} else {
    "FAIL: WN11-CC-000005 is not configured correctly. Current value: $Value"
}

# reg.exe confirmation
reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows\Personalization" /v NoLockScreenCamera

 Expected result:
 NoLockScreenCamera    REG_DWORD    0x1
