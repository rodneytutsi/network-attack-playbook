# 📄 ARP Spoofing

| Field | Details |
|-------|---------|
| **Category** | LAN / Man-in-the-Middle |
| **MITRE Tactic** | Collection |
| **MITRE Technique** | T1557.002 – ARP Cache Poisoning |
| **Severity** | 🔴 High |
| **Protocols Affected** | ARP, IPv4 |

---

## 1. What Is It?

ARP Spoofing (also called ARP Poisoning) is a network attack where an attacker sends **fake ARP (Address Resolution Protocol) messages** onto a local network. The goal is to link the attacker's MAC address with the IP address of a legitimate host (such as the default gateway), causing traffic meant for that host to be sent to the attacker instead.

This is one of the most common **Man-in-the-Middle (MitM)** attack techniques on local area networks (LANs).

---

## 2. How It Works

### Background: What is ARP?
ARP is a protocol used to map **IP addresses to MAC addresses** on a local network. Every device maintains an **ARP cache** — a table of IP-to-MAC mappings.

### Attack Steps

```
Step 1: Attacker joins the same LAN as the victim and gateway.

Step 2: Attacker sends fake ARP Reply packets:
        → To the VICTIM:  "The gateway (192.168.1.1) is at MY MAC address"
        → To the GATEWAY: "The victim (192.168.1.50) is at MY MAC address"

Step 3: Both victim and gateway update their ARP caches with false entries.

Step 4: All traffic between victim and gateway now flows through the attacker.
        Attacker can: Read, modify, or drop the traffic (MitM position achieved).
```

### Diagram
```
Normal Traffic:
Victim ──────────────────────────► Gateway ──► Internet

After ARP Spoofing:
Victim ──► Attacker (intercepts) ──► Gateway ──► Internet
                    ↑
              (reads/modifies traffic)
```

---

## 3. Real-World Example

In 2015, attackers used ARP spoofing within hotel networks to intercept guests' unencrypted traffic, capturing login credentials and session cookies. This type of attack is extremely common on public Wi-Fi and corporate LAN environments where ARP is not secured.

---

## 4. Tools Used by Attackers

| Tool | Description |
|------|-------------|
| **Arpspoof** (dsniff) | Classic CLI tool for sending fake ARP packets |
| **Ettercap** | Full-featured MitM framework with ARP spoofing support |
| **Bettercap** | Modern, powerful MitM toolkit |
| **Scapy** (Python) | Craft custom ARP packets manually |

---

## 5. Detection Methods

| Method | How It Detects ARP Spoofing |
|--------|-----------------------------|
| **ARP cache monitoring** | Watch for IP addresses that suddenly change their MAC address mapping |
| **Wireshark / packet capture** | Look for duplicate ARP replies for the same IP from different MACs |
| **IDS/IPS tools** | Tools like Snort or Suricata have ARP spoofing detection signatures |
| **XArp (Windows tool)** | Dedicated ARP spoofing detection tool |
| **SIEM alerts** | Alert on rapid ARP reply floods or gratuitous ARP anomalies |

### Wireshark Detection Filter:
```
arp.duplicate-address-detected
```

---

## 6. Defensive Measures

| Defense | Description |
|---------|-------------|
| **Dynamic ARP Inspection (DAI)** | Switch-level feature that validates ARP packets against a DHCP snooping table |
| **Static ARP entries** | Manually set ARP entries for critical devices (gateway) — prevents overwriting |
| **VLAN segmentation** | Limit the broadcast domain — reduces the number of hosts an attacker can reach |
| **Encrypted protocols** | Use HTTPS, SSH, TLS — even if intercepted, traffic is unreadable |
| **802.1X Port Authentication** | Authenticate devices before allowing LAN access |
| **Network monitoring** | Continuously monitor ARP tables for anomalies |

---

## 7. MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| **Tactic** | Collection |
| **Technique** | T1557 – Adversary-in-the-Middle |
| **Sub-technique** | T1557.002 – ARP Cache Poisoning |
| **Link** | https://attack.mitre.org/techniques/T1557/002/ |

---

## 8. References

- MITRE ATT&CK T1557.002: https://attack.mitre.org/techniques/T1557/002/
- Cisco – Dynamic ARP Inspection: https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst6500/ios/12-2SX/configuration/guide/book/dai.html
- Bettercap Documentation: https://www.bettercap.org
