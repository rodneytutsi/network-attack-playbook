# 📄 DNS Poisoning

| Field | Details |
|-------|---------|
| **Category** | Protocol Abuse |
| **MITRE Tactic** | Impact |
| **MITRE Technique** | T1584.002 – DNS Server |
| **Severity** | 🔴 High |
| **Protocols Affected** | DNS, UDP/53, TCP/53 |

---

## 1. What Is It?

DNS Poisoning (also called DNS Cache Poisoning or DNS Spoofing) is an attack where **corrupt DNS data is injected into a DNS resolver's cache**. This causes the resolver to return a **fraudulent IP address** for a domain name, redirecting users to malicious servers without their knowledge.

Since DNS is the "phone book of the internet," poisoning it allows attackers to silently redirect entire populations of users to fake websites — enabling phishing, credential theft, and malware delivery.

---

## 2. How It Works

### Background: How Does DNS Work?
When you type `www.bank.com`:
1. Your device asks a **DNS resolver** for the IP address
2. The resolver checks its **cache** — if found, returns it immediately
3. If not cached, it queries authoritative DNS servers and **caches the result**

### Attack Steps

```
Step 1: Attacker identifies a target DNS resolver with no source port randomization
        or weak transaction ID validation.

Step 2: Attacker floods the resolver with fake DNS responses containing:
        → Correct domain name (e.g., www.bank.com)
        → Attacker's malicious IP address
        → A matching (or guessed) transaction ID

Step 3: If the fake response arrives before the real one, the resolver
        accepts and CACHES the malicious IP.

Step 4: All users querying that resolver for www.bank.com are sent
        to the attacker's server — unknowingly.
```

### Diagram
```
Normal DNS Resolution:
User → DNS Resolver → Authoritative DNS → Returns: bank.com = 192.0.2.1 ✅

After DNS Poisoning:
User → DNS Resolver (poisoned cache) → Returns: bank.com = 10.0.0.99 ❌ (attacker's server)
```

---

## 3. Real-World Example

**2008 – Kaminsky Attack:** Security researcher Dan Kaminsky discovered a critical flaw in DNS allowing mass cache poisoning. Attackers could poison resolvers within seconds by exploiting predictable transaction IDs and source ports. This led to emergency patching across the entire internet DNS infrastructure and accelerated adoption of DNSSEC.

---

## 4. Tools Used by Attackers

| Tool | Description |
|------|-------------|
| **Ettercap** | Includes DNS spoofing plugin for LAN-based attacks |
| **Bettercap** | DNS spoofing module for MitM scenarios |
| **dnschef** | Flexible DNS proxy for spoofing responses |
| **Scapy** | Craft and inject custom DNS packets |
| **MitmProxy** | Intercept and modify DNS traffic |

---

## 5. Detection Methods

| Method | Details |
|--------|---------|
| **DNSSEC validation** | Cryptographically signed DNS records — forged records fail validation |
| **DNS response monitoring** | Alert on unexpected changes in resolved IPs for known domains |
| **TTL anomalies** | Poisoned entries often have unusual TTL values |
| **Passive DNS logging** | Log all DNS responses and compare against known-good baselines |
| **SIEM correlation** | Alert when a domain resolves to a new, unknown IP |

---

## 6. Defensive Measures

| Defense | Description |
|---------|-------------|
| **DNSSEC** | Digitally signs DNS records — prevents forged responses from being accepted |
| **DNS over HTTPS (DoH)** | Encrypts DNS queries — prevents interception and tampering |
| **DNS over TLS (DoT)** | Encrypts DNS transport layer |
| **Source port randomization** | Makes it harder to guess transaction IDs (implemented post-Kaminsky) |
| **Short TTLs + monitoring** | Detect cache changes quickly |
| **Use trusted resolvers** | Cloudflare (1.1.1.1), Google (8.8.8.8) with DoH/DoT support |

---

## 7. MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| **Tactic** | Impact |
| **Technique** | T1584.002 – Compromise Infrastructure: DNS Server |
| **Related** | T1557 – Adversary-in-the-Middle |
| **Link** | https://attack.mitre.org/techniques/T1584/002/ |

---

## 8. References

- MITRE ATT&CK T1584.002: https://attack.mitre.org/techniques/T1584/002/
- Kaminsky, D. (2008). *Black Ops 2008: It's The End Of The Cache As We Know It.* DEF CON 16.
- Cloudflare – What is DNS Cache Poisoning?: https://www.cloudflare.com/learning/dns/dns-cache-poisoning/
- RFC 4033 – DNS Security Introduction and Requirements (DNSSEC)
