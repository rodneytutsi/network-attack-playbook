# 📄 Man-in-the-Middle (MitM) Attack

| Field | Details |
|-------|---------|
| **Category** | Interception |
| **MITRE Tactic** | Collection |
| **MITRE Technique** | T1557 – Adversary-in-the-Middle |
| **Severity** | 🔴 High |
| **Protocols Affected** | HTTP, HTTPS, DNS, ARP, SSL/TLS |

---

## 1. What Is It?

A Man-in-the-Middle (MitM) attack occurs when an attacker **secretly positions themselves between two communicating parties** — intercepting, reading, and potentially modifying the data being exchanged. Neither the victim nor the server realizes a third party is involved.

MitM is not a single technique but a **class of attacks** that can be achieved through various methods including ARP Spoofing, DNS Poisoning, SSL Stripping, and rogue Wi-Fi access points.

---

## 2. How It Works

### Attack Steps
```
Step 1: Attacker gains a position between victim and destination.
        Methods: ARP Spoofing, Rogue Wi-Fi, DNS Poisoning, BGP Hijacking

Step 2: Victim's traffic flows through attacker's machine.

Step 3: Attacker can:
        → INTERCEPT — read usernames, passwords, session tokens
        → MODIFY    — alter web pages, inject malicious content
        → REPLAY    — reuse captured authentication tokens
        → STRIP     — downgrade HTTPS to HTTP (SSL Stripping)
```

### Diagram
```
Normal Communication:
Victim ◄──────────────────────► Server

MitM Attack:
Victim ◄──► Attacker ◄──► Server
              ↑
    (reads and/or modifies all data)
```

---

## 3. Common MitM Techniques

| Technique | How It's Done |
|-----------|--------------|
| **ARP Spoofing** | Poison ARP cache to redirect LAN traffic |
| **DNS Spoofing** | Return fake IP for domain lookups |
| **SSL Stripping** | Downgrade HTTPS connections to HTTP |
| **Rogue Access Point** | Set up a fake Wi-Fi hotspot to intercept wireless traffic |
| **BGP Hijacking** | Corrupt internet routing tables (nation-state level) |
| **Evil Twin Attack** | Clone a legitimate Wi-Fi SSID to lure victims |

---

## 4. Real-World Example

**2011 – DigiNotar CA Compromise:** Attackers compromised the Dutch certificate authority DigiNotar and issued fraudulent SSL certificates for Google.com. Iranian users were subjected to MitM attacks where their encrypted Gmail traffic was intercepted using the forged certificate. DigiNotar collapsed as a result.

---

## 5. Tools Used by Attackers

| Tool | Description |
|------|-------------|
| **Bettercap** | All-in-one MitM framework (ARP, DNS, SSL Strip) |
| **Ettercap** | Classic LAN MitM tool |
| **MitmProxy** | Intercept and inspect HTTPS traffic |
| **SSLStrip** | Downgrade HTTPS to HTTP automatically |
| **Wireshark** | Capture and analyze intercepted traffic |
| **Aircrack-ng** | Wireless MitM and rogue AP attacks |

---

## 6. Detection Methods

| Method | Details |
|--------|---------|
| **Certificate pinning** | Apps reject certificates that don't match a known pinned cert |
| **HSTS (HTTP Strict Transport Security)** | Browser refuses HTTP connections to known HTTPS sites |
| **ARP monitoring** | Detect ARP cache changes indicative of spoofing |
| **SSL certificate inspection** | Verify certificate issuer and validity |
| **Network traffic analysis** | Unexpected latency or routing changes |
| **Endpoint detection tools** | EDR solutions can detect MitM-related process behavior |

---

## 7. Defensive Measures

| Defense | Description |
|---------|-------------|
| **Use HTTPS everywhere** | Encrypts traffic so interception yields ciphertext |
| **HSTS Preloading** | Forces browsers to always use HTTPS for known domains |
| **VPN usage** | Encrypts all traffic regardless of network |
| **Certificate Transparency (CT)** | Public log of issued certificates — detects fraudulent certs |
| **Multi-Factor Authentication (MFA)** | Stolen credentials alone are not enough to log in |
| **Network segmentation** | Limits attacker's reach on the LAN |
| **Public Wi-Fi awareness** | Avoid sensitive transactions on untrusted networks |

---

## 8. MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| **Tactic** | Collection |
| **Technique** | T1557 – Adversary-in-the-Middle |
| **Sub-techniques** | T1557.001 (LLMNR/NBT-NS), T1557.002 (ARP Cache Poisoning) |
| **Link** | https://attack.mitre.org/techniques/T1557/ |

---

## 9. References

- MITRE ATT&CK T1557: https://attack.mitre.org/techniques/T1557/
- Cloudflare – What is a MitM Attack?: https://www.cloudflare.com/learning/security/threats/man-in-the-middle-attack/
- DigiNotar Incident Report (2011) – Fox-IT
- Bettercap Documentation: https://www.bettercap.org
