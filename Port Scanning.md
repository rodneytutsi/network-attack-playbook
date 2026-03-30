# 📄 Port Scanning (Reconnaissance)

| Field | Details |
|-------|---------|
| **Category** | Reconnaissance |
| **MITRE Tactic** | Discovery |
| **MITRE Technique** | T1046 – Network Service Discovery |
| **Severity** | 🟡 Low (on its own) |
| **Protocols Affected** | TCP, UDP, ICMP |

---

## 1. What Is It?

Port scanning is a technique used to **discover open ports and services** running on a target host or network. While it is a legitimate network administration tool, attackers use it as a **reconnaissance step** to map the target environment before launching further attacks.

An open port is a potential entry point. Knowing which ports are open tells the attacker which services are running — and which vulnerabilities may exist.

---

## 2. How It Works

### Background: What Are Ports?
Every networked application listens on a specific **port number** (0–65535). For example:
- Port 22 → SSH
- Port 80 → HTTP
- Port 443 → HTTPS
- Port 3389 → RDP (Remote Desktop)

### Common Scan Types

| Scan Type | How It Works | Stealth Level |
|-----------|-------------|---------------|
| **TCP Connect Scan** | Completes full 3-way handshake | Low (easily logged) |
| **SYN Scan (Half-open)** | Sends SYN, doesn't complete handshake | Medium |
| **UDP Scan** | Sends UDP packets, listens for response | Medium |
| **NULL Scan** | Sends packet with no flags | High |
| **FIN Scan** | Sends FIN flag only | High |
| **Xmas Scan** | Sends FIN, PSH, URG flags | High |
| **OS Detection** | Analyzes TCP/IP stack responses | Varies |

### Attack Steps
```
Step 1: Attacker identifies target IP range or host.

Step 2: Runs port scanner (e.g., Nmap) to probe each port.

Step 3: Analyzes results:
        → OPEN   : Service is listening — potential attack surface
        → CLOSED : No service, but host is alive
        → FILTERED: Firewall is blocking — port may still exist

Step 4: Cross-references open ports with known vulnerabilities
        (e.g., Port 445 open → check for EternalBlue/MS17-010).
```

---

## 3. Real-World Example

Before every major cyberattack, adversaries perform port scanning. In the **2017 Equifax breach**, attackers used reconnaissance tools to map the network and identify an unpatched Apache Struts server (Port 443/HTTPS) before exploiting CVE-2017-5638 to gain initial access.

---

## 4. Tools Used by Attackers

| Tool | Description |
|------|-------------|
| **Nmap** | Industry-standard port scanner — open source |
| **Masscan** | Extremely fast internet-wide scanner |
| **Shodan** | Search engine for internet-exposed devices and services |
| **Angry IP Scanner** | Lightweight GUI-based scanner |
| **Netcat (nc)** | Manual port connection testing |

### Example Nmap Commands (for authorized use only):
```bash
# Basic scan of top 1000 ports
nmap 192.168.1.1

# Full port scan
nmap -p- 192.168.1.1

# Service version detection
nmap -sV 192.168.1.1

# OS detection + version + script scan
nmap -A 192.168.1.1

# Stealthy SYN scan
nmap -sS 192.168.1.1
```

---

## 5. Detection Methods

| Method | Details |
|--------|---------|
| **IDS/IPS rules** | Snort/Suricata detect port scan patterns (many connections in short time) |
| **Firewall logs** | Repeated connection attempts from a single IP to multiple ports |
| **SIEM correlation** | Alert on sequential port access or scan-like behavior |
| **Honeypots** | Set up fake services on unused ports — any connection is suspicious |
| **Rate-based detection** | Flag IPs that probe more than N ports per second |

---

## 6. Defensive Measures

| Defense | Description |
|---------|-------------|
| **Firewall rules** | Block all ports not required for business operations |
| **Default deny policy** | Only allow explicitly needed services |
| **Port knocking** | Services only open after a secret sequence of connection attempts |
| **IDS/IPS deployment** | Detect and block scanning activity |
| **Disable unused services** | Reduce the attack surface — fewer open ports = fewer risks |
| **Egress filtering** | Prevent internal scanning from reaching the internet |

---

## 7. MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| **Tactic** | Discovery |
| **Technique** | T1046 – Network Service Discovery |
| **Link** | https://attack.mitre.org/techniques/T1046/ |

---

## 8. References

- MITRE ATT&CK T1046: https://attack.mitre.org/techniques/T1046/
- Nmap Official Documentation: https://nmap.org/book/
- Shodan: https://www.shodan.io
- Equifax Breach Analysis (2017) – US House of Representatives Report
