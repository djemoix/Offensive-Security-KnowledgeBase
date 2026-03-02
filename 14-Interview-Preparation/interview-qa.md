# Interview Preparation – Cybersecurity Q&A
**Author: Manish Verma**

---

## SOC Analyst Interview Questions

### Q: What is the difference between IDS and IPS?
**A:** IDS (Intrusion Detection System) monitors and alerts on suspicious activity but does not block it. IPS (Intrusion Prevention System) monitors AND actively blocks/prevents detected threats inline.

### Q: What is the CIA Triad?
**A:** Confidentiality (data only accessible to authorized users), Integrity (data accuracy and completeness maintained), Availability (systems and data accessible when needed).

### Q: Walk me through your incident response process.
**A:** Detection → Analysis → Containment → Eradication → Recovery → Post-Incident Review (following NIST or SANS IR lifecycle).

### Q: What is a SIEM and how do you use it?
**A:** Security Information and Event Management. Collects logs from multiple sources, correlates events, and generates alerts. I use Graylog/Splunk to write detection rules, investigate alerts, and create dashboards for monitoring.

---

## Active Directory Questions

### Q: Explain Kerberos authentication.
**A:**
1. Client requests TGT from KDC (AS-REQ)
2. KDC issues encrypted TGT (AS-REP) 
3. Client uses TGT to request Service Ticket (TGS-REQ)
4. KDC issues Service Ticket (TGS-REP)
5. Client presents Service Ticket to service
Key: TGT encrypted with krbtgt hash, Service Ticket encrypted with service account hash.

### Q: What is Kerberoasting?
**A:** Attack targeting service accounts with SPNs. Attacker requests service tickets (TGS) for those accounts, which are encrypted with the service account's NTLM hash. The tickets can be cracked offline to recover plaintext passwords.

### Q: NTLM vs Kerberos?
**A:**
- NTLM: Challenge-response, older, doesn't require domain controller contact after initial auth, used when Kerberos unavailable
- Kerberos: Ticket-based, more secure, requires DC contact, supports mutual authentication, default in AD environments

### Q: What is Pass-the-Hash?
**A:** Technique using captured NTLM hash (instead of plaintext password) to authenticate to services. Attacker uses hash directly without cracking it.

---

## Penetration Testing Questions

### Q: What are the phases of a penetration test?
**A:**
1. Pre-engagement (scope, rules of engagement)
2. Reconnaissance (passive and active)
3. Scanning & Enumeration
4. Exploitation
5. Post-Exploitation (pivoting, persistence)
6. Reporting

### Q: What is the difference between vulnerability assessment and penetration testing?
**A:** Vulnerability assessment identifies and catalogues vulnerabilities without exploiting them. Penetration testing goes further by actually exploiting vulnerabilities to demonstrate real-world impact.

### Q: How do you escalate privileges on Linux?
**A:** Check SUID binaries, sudo permissions, cron jobs, writable files, capabilities, PATH manipulation, kernel exploits. Tools: LinPEAS, LinEnum.

---

## Incident Response Lifecycle
```
NIST Framework:
1. Preparation - Policies, tools, training
2. Detection & Analysis - Identify incident, assess severity
3. Containment - Short-term and long-term containment
4. Eradication - Remove threat, patch vulnerability
5. Recovery - Restore systems, verify
6. Post-Incident Activity - Lessons learned, documentation
```

---

## Risk & Compliance

### Q: Explain NIST Cybersecurity Framework.
**A:** Five core functions: Identify → Protect → Detect → Respond → Recover. Framework for managing cybersecurity risk.

### Q: What is ISO 27001?
**A:** International standard for Information Security Management Systems (ISMS). Provides framework for establishing, implementing, maintaining, and improving information security.
