# Port-Wise Enumeration Playbook
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Full port enumeration for penetration testing  
**Tools Used:** Nmap, Netcat, Enum4linux, Gobuster, Nikto  
**Outcome:** Service identification and attack surface mapping

---

## Port 21 – FTP

### Checklist
- [ ] Anonymous login
- [ ] Version detection
- [ ] Writable directories
- [ ] Binary files

### Commands
```bash
nmap -sV -p 21 --script ftp-anon,ftp-syst,ftp-vsftpd-backdoor <IP>
ftp <IP>                        # Try anonymous:anonymous
hydra -l admin -P rockyou.txt ftp://<IP>
```

---

## Port 22 – SSH

### Checklist
- [ ] Version (check for old vulnerable versions)
- [ ] Password auth enabled
- [ ] Key auth
- [ ] Username enumeration

### Commands
```bash
nmap -sV -p 22 --script ssh-auth-methods,ssh-hostkey <IP>
ssh <user>@<IP>
hydra -l root -P rockyou.txt ssh://<IP>
ssh-audit <IP>
```

---

## Port 25 – SMTP

### Checklist
- [ ] VRFY username enumeration
- [ ] Open relay
- [ ] Version banner

### Commands
```bash
nmap -p 25 --script smtp-enum-users,smtp-open-relay <IP>
nc -nv <IP> 25
VRFY root
```

---

## Port 80/443 – HTTP/HTTPS

### Checklist
- [ ] Web technology fingerprint
- [ ] Directory enumeration
- [ ] CMS detection
- [ ] SSL certificate info

### Commands
```bash
nmap -sV -p 80,443 --script http-title,http-headers <IP>
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
nikto -h http://<IP>
whatweb http://<IP>
```

---

## Port 139/445 – SMB

### Checklist
- [ ] Null session
- [ ] Shares enumeration
- [ ] Users enumeration
- [ ] Password policy
- [ ] MS17-010 (EternalBlue)

### Commands
```bash
nmap -p 445 --script smb-vuln-ms17-010,smb-enum-shares,smb-enum-users <IP>
enum4linux -a <IP>
smbclient -L //<IP>/ -N
smbmap -H <IP>
crackmapexec smb <IP>
```

---

## Port 389 – LDAP

### Checklist
- [ ] Anonymous bind
- [ ] Domain info
- [ ] User enumeration

### Commands
```bash
nmap -p 389 --script ldap-rootdse,ldap-search <IP>
ldapsearch -x -h <IP> -b "dc=domain,dc=com"
```

---

## Port 3306 – MySQL

### Checklist
- [ ] Default credentials
- [ ] Remote access
- [ ] Database enumeration

### Commands
```bash
nmap -p 3306 --script mysql-info,mysql-enum <IP>
mysql -h <IP> -u root -p
hydra -l root -P rockyou.txt mysql://<IP>
```

---

## Port 3389 – RDP

### Checklist
- [ ] BlueKeep vulnerability
- [ ] NLA enabled
- [ ] Brute force

### Commands
```bash
nmap -p 3389 --script rdp-vuln-ms12-020,rdp-enum-encryption <IP>
xfreerdp /u:admin /p:password /v:<IP>
hydra -l admin -P rockyou.txt rdp://<IP>
```

---

## ⚠️ Disclaimer
All techniques for authorized testing only.
