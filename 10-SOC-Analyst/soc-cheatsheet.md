# SOC Analyst Cheat Sheet
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Security Operations Center analysis and incident response  
**Tools Used:** Graylog, Splunk, Wireshark, Zeek  
**Outcome:** Detect, investigate, and respond to security incidents

---

## Key Windows Event IDs

| Event ID | Description | Importance |
|----------|-------------|------------|
| 4624 | Successful logon | Medium |
| 4625 | Failed logon | High |
| 4648 | Logon with explicit credentials | High |
| 4672 | Special privileges assigned | High |
| 4688 | Process creation | High |
| 4698 | Scheduled task created | High |
| 4720 | User account created | High |
| 4728 | Member added to security group | High |
| 4732 | Member added to local group | High |
| 4756 | Member added to universal group | Medium |
| 4769 | Kerberos service ticket request | High |
| 4771 | Kerberos pre-auth failed | High |
| 4776 | NTLM auth attempt | High |
| 7045 | New service installed | Critical |

---

## SIEM Query Examples

### Splunk
```
# Failed logons
index=wineventlog EventCode=4625 | stats count by src_ip, user | sort -count

# Suspicious process
index=wineventlog EventCode=4688 New_Process_Name="*powershell*" | table _time, user, host, New_Process_Name, Process_Command_Line

# Lateral movement
index=wineventlog EventCode=4624 Logon_Type=3 | stats count by src_ip, dest_host | sort -count
```

### Graylog
```
# Brute force detection
event_id:4625 AND timestamp:[now-1h TO now]

# Admin account usage
event_id:4624 AND account_name:Administrator

# Suspicious PowerShell
event_id:4688 AND new_process_name:*powershell*
```

---

## Incident Response Workflow

```
1. DETECTION
   - Alert triggered in SIEM
   - Identify affected systems
   
2. ANALYSIS  
   - Collect logs
   - Timeline reconstruction
   - IOC extraction (IPs, hashes, domains)
   
3. CONTAINMENT
   - Isolate affected system
   - Block malicious IPs/domains
   - Disable compromised accounts
   
4. ERADICATION
   - Remove malware
   - Patch vulnerability
   - Reset credentials
   
5. RECOVERY
   - Restore from clean backup
   - Monitor for re-infection
   
6. LESSONS LEARNED
   - Document incident
   - Update detection rules
   - Improve defenses
```

---

## Common IOC Types

```
Network: IP addresses, domains, URLs, port numbers
Host: File hashes (MD5/SHA256), file paths, registry keys
Email: Sender address, subject, attachment hashes
Behavior: Process chains, network connections, persistence mechanisms
```

---

## ⚠️ Note
All investigation techniques for authorized SOC operations only.
