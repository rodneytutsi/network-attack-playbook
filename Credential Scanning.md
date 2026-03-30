# 📄 Credential Sniffing

| Field | Details |
|-------|---------|
| **Category** | Passive Attack |
| **MITRE Tactic** | Credential Access |
| **MITRE Technique** | T1040 – Network Sniffing |
| **Severity** | 🔴 High |
| **Protocols Affected** | HTTP, FTP, TELNET, SMTP, POP3, IMAP |

---

## 1. What Is It?

Credential sniffing is a **passive network attack** where an attacker captures network packets to extract **usernames, passwords, and session tokens** transmitted in plaintext. Unlike active attacks, sniffing can be completely silent — no packets are sent, no connections are made, making it very difficult to detect.

It is most dangerous on unencrypted protocols and unsecured networks (public Wi-Fi, legacy systems).

---

## 2. How It Works

### Background: Promiscuous Mode
Network interfaces normally only process packets addressed to them. In **promiscuous mode**, a network card captures **all packets** on the network segment — including those meant for other devices.

### Attack Steps
```
Step 1: Attacker places their network interface in promiscuous mode.

Step 2: Attacker uses a packet capture tool (e.g., Wireshark, tcpdump)
        to record all network traffic on the segment.

Step 3: Attacker filters captured traffic for authentication protocols:
        → HTTP POST requests (login forms)
        → FTP (username/password sent in cleartext)
        → TELNET (all keystrokes captured)
        → SMTP/POP3/IMAP without TLS (email credentials)

Step 4: Attacker extracts credentials from the captured packets.

Step 5: Credentials are used for unauthorized access (account takeover).
```

### On Switched Networks (Additional Step Required)
Modern switches only send traffic to the intended port, not all ports. To sniff on a switched network, the attacker must first:
- **ARP Spoof** to redirect traffic through their machine, OR
- Compromise a **network tap** or **SPAN port**, OR
- Use a **rogue access point**

---

## 3. Real-World Example

In 2014, the **POODLE attack** demonstrated how attackers could downgrade SSL connections to the older SSLv3 protocol and then sniff credentials from the now-weakly-encrypted traffic. This affected millions of HTTPS connections and forced the deprecation of SSLv3 across the internet.

---

## 4. Tools Used by Attackers

| Tool | Description |
|------|-------------|
| **Wireshark** | Graphical packet analyzer — can filter for credentials |
| **tcpdump** | CLI packet capture tool |
| **dsniff** | Specialized tool for sniffing passwords from protocols |
| **Ettercap** | MitM + sniffing combined toolkit |
| **Bettercap** | Modern sniffing and credential harvesting framework |
| **Cain & Abel** | Windows-based credential sniffer (legacy) |

### Wireshark Filter to Find HTTP Credentials:
```
http.request.method == "POST"
```

---

## 5. Detection Methods

| Method | Details |
|--------|---------|
| **Promiscuous mode detection** | Tools like `arpwatch` can detect NICs in promiscuous mode |
| **Network traffic baselining** | Unexpected traffic patterns from a host may indicate sniffing |
| **Honeypot credentials** | Plant fake credentials in traffic — if used, attacker is detected |
| **Encrypted protocol enforcement** | If all traffic is encrypted, sniffed data is useless |
| **IDS signatures** | Detect known sniffing tool fingerprints on the network |

---

## 6. Defensive Measures

| Defense | Description |
|---------|-------------|
| **Use encrypted protocols only** | HTTPS instead of HTTP, SFTP instead of FTP, SSH instead of Telnet |
| **TLS/SSL everywhere** | Encrypt all application-layer communications |
| **VPN for sensitive communication** | Tunnel all traffic through encrypted VPN |
| **Network segmentation** | Limit which hosts can see which traffic |
| **802.1X authentication** | Authenticate devices before allowing network access |
| **Disable legacy protocols** | Disable FTP, Telnet, POP3 without TLS on all systems |
| **MFA** | Even if credentials are captured, MFA blocks unauthorized access |

---

## 7. MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| **Tactic** | Credential Access |
| **Technique** | T1040 – Network Sniffing |
| **Link** | https://attack.mitre.org/techniques/T1040/ |

---

## 8. References

- MITRE ATT&CK T1040: https://attack.mitre.org/techniques/T1040/
- Wireshark Documentation: https://www.wireshark.org/docs/
- POODLE Attack – CVE-2014-3566: https://nvd.nist.gov/vuln/detail/CVE-2014-3566
- SANS – Network Sniffing and Defense
