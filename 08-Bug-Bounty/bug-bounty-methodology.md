# Bug Bounty Methodology
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Public bug bounty programs (HackerOne, Bugcrowd, etc.)  
**Tools Used:** Amass, Subfinder, httpx, Nuclei, Burp Suite  
**Outcome:** Report valid security vulnerabilities for reward

---

## Phase 1: Recon

```bash
# Subdomain enumeration
subfinder -d target.com -o subdomains.txt
amass enum -passive -d target.com
assetfinder --subs-only target.com

# Probe live hosts
cat subdomains.txt | httpx -status-code -title -tech-detect -o live_hosts.txt

# Screenshot all subdomains
gowitness file -f live_hosts.txt
```

---

## Phase 2: JS Analysis

```bash
# Extract endpoints from JS files
echo "https://target.com" | gau | grep ".js" | sort -u | tee js_files.txt
cat js_files.txt | xargs -I% sh -c 'curl -sk "%" | grep -oP "[\/](api|v1|v2)[^\"]*"'

# Look for:
# - API keys / tokens
# - Internal endpoints
# - S3 bucket URLs
# - Hidden parameters
```

---

## Phase 3: IDOR Testing

```
Checklist:
- [ ] Change numeric IDs in URL/body
- [ ] Try other user's resource IDs
- [ ] Test GUIDs (sometimes predictable)
- [ ] Horizontal privilege escalation
- [ ] Vertical privilege escalation
- [ ] Mass assignment vulnerabilities
```

---

## Phase 4: Business Logic Flaws

```
Test cases:
- [ ] Negative values in payment fields
- [ ] Skip payment step
- [ ] Rate limit bypass (headers, IP rotation)
- [ ] Coupon code stacking
- [ ] Account takeover via email change
- [ ] Password reset token reuse
- [ ] 2FA bypass
```

---

## Phase 5: Common High-Impact Bugs

```
SSRF:
- Test URL parameters: ?url=, ?redirect=, ?next=
- Internal: http://169.254.169.254/latest/meta-data/

XSS:
- Reflected in search, error params
- Stored in profile, comments
- DOM-based in JS sinks

SQLi:
- ' OR 1=1--
- ' UNION SELECT 1,2,3--
- Time-based: ' AND SLEEP(5)--

Open Redirect:
- ?redirect=https://evil.com
- ?next=//evil.com
```

---

## Tools Summary

```bash
subfinder  → subdomain discovery
httpx      → probe live hosts
nuclei     → automated vulnerability scan
gau        → get all URLs
ffuf       → web fuzzing
sqlmap     → SQL injection
dalfox     → XSS scanner
```

---

## ⚠️ Disclaimer
Only test on authorized programs with proper scope.
