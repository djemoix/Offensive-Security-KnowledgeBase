# Blue Team Detection Notes
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Defensive security - attack detection and mitigation  
**Tools Used:** SIEM, EDR, IDS/IPS, Windows Event Logs  
**Outcome:** Map attacks to detection logic and mitigation strategies

---

## Attack → Detection Mapping

| Attack Technique | Event ID | Detection Logic | Mitigation |
|-----------------|----------|-----------------|------------|
| Password Spray | 4625 | Multiple failed logins from same IP to different users | Account lockout policy, MFA |
| Kerberoasting | 4769 | High volume service ticket requests (RC4 encryption) | Use strong service account passwords, AES encryption |
| AS-REP Roasting | 4768 | TGT request without preauth | Enable preauth on all accounts |
| Pass-the-Hash | 4624 (Type 3) | Logon type 3 with NTLM, unusual source | Credential Guard, LAPS |
| DCSync | 4662 | Non-DC replication requests | Restrict replication rights |
| Lateral Movement (PSExec) | 7045 + 4624 | New service + remote logon | Host firewall, EDR |
| Golden Ticket | 4769 | Kerberos tickets with unusual lifetime | Monitor krbtgt resets |
| LLMNR Poisoning | Network | LLMNR/NBT-NS responses from non-DC | Disable LLMNR/NBT-NS |
| Mimikatz | 4688 | lsass.exe access, sekurlsa commands | Credential Guard, AV/EDR |
| Scheduled Task | 4698 | New task created, especially at system level | Audit scheduled tasks |

---

## Detection Logic Examples

### Brute Force / Password Spray
```
Threshold: >10 failed logins in 5 minutes from same IP
Event: 4625
Alert: Multiple accounts targeted from single source
```

### Suspicious PowerShell Execution
```
Event: 4688 (process creation)
Indicators:
- powershell.exe with -enc flag (base64)
- powershell.exe downloading from internet
- powershell.exe spawned from Office apps
- Hidden window (-w hidden -nop)
```

### Lateral Movement Detection
```
Event: 4624 (Type 3) + 4776
Indicators:
- Logon from workstation to workstation (not server)
- Non-admin accounts accessing admin shares
- Unusual service installations
```

---

## Hardening Checklist

```
Active Directory:
- [ ] Enable Kerberos preauth on all accounts
- [ ] Use strong service account passwords (25+ chars)
- [ ] Implement LAPS for local admin passwords
- [ ] Disable LLMNR and NBT-NS
- [ ] Enable audit policies (logon, object access, process)
- [ ] Implement Privileged Access Workstations (PAW)
- [ ] Tier model for admin accounts

Windows:
- [ ] Enable Windows Defender Credential Guard
- [ ] Disable WDigest authentication
- [ ] Enable PowerShell logging
- [ ] Apply LSASS protection
- [ ] Enable Attack Surface Reduction rules
```

---

## Purple Team Mindset

```
Run attack → Check if detection triggers → Tune alert → Improve detection

Key principle:
If you can simulate it (Red Team), you can detect it (Blue Team)
```

---

## ⚠️ Note
All detection techniques for authorized defensive security operations.
