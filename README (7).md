# 📄 VLAN Hopping

| Field | Details |
|-------|---------|
| **Category** | Network Evasion / Lateral Movement |
| **MITRE Tactic** | Lateral Movement |
| **MITRE Technique** | T1599 – Network Boundary Bridging |
| **Severity** | 🟠 Medium |
| **Protocols Affected** | 802.1Q, DTP (Dynamic Trunking Protocol) |

---

## 1. What Is It?

VLAN Hopping is an attack that allows an attacker to **send traffic to a VLAN that they should not have access to**, bypassing network segmentation controls. VLANs (Virtual Local Area Networks) are used to logically separate network segments — but misconfigurations can allow attackers to "hop" between them.

There are two primary methods: **Switch Spoofing** and **Double Tagging**.

---

## 2. How It Works

### Background: What Is a VLAN?
A VLAN separates a physical network into multiple logical networks. For example:
- VLAN 10 → Finance department
- VLAN 20 → HR department
- VLAN 30 → IT department

Normally, hosts on VLAN 10 cannot communicate with VLAN 20 without going through a router (where access controls are enforced).

---

### Method 1: Switch Spoofing

```
Step 1: Many switches run DTP (Dynamic Trunking Protocol) by default,
        which auto-negotiates trunk links.

Step 2: Attacker configures their NIC to send DTP negotiation packets,
        tricking the switch into forming a TRUNK link with their device.

Step 3: As a trunk port, attacker's device receives traffic from ALL VLANs
        and can send traffic tagged for any VLAN.
```

### Method 2: Double Tagging

```
Step 1: Attacker is on the same VLAN as the switch's native VLAN (e.g., VLAN 1).

Step 2: Attacker crafts a packet with TWO 802.1Q VLAN tags:
        → Outer tag: Native VLAN (e.g., VLAN 1)
        → Inner tag: Target VLAN (e.g., VLAN 20)

Step 3: First switch strips the outer tag (native VLAN) and forwards
        the packet with the inner tag (VLAN 20) intact.

Step 4: Second switch sees the inner VLAN 20 tag and delivers the packet
        to a host on VLAN 20 — which the attacker should not reach.

Note: Double tagging is ONE-WAY only (attacker can send but not receive replies).
```

### Diagram (Double Tagging)
```
Attacker (VLAN 1)
  Packet: [VLAN1 tag][VLAN20 tag][Data]
         ↓
Switch 1: Strips VLAN1 tag → forwards [VLAN20 tag][Data]
         ↓
Switch 2: Sees VLAN20 → delivers to VLAN 20 host ✅ (unauthorized access)
```

---

## 3. Real-World Example

VLAN hopping via DTP switch spoofing has been demonstrated in numerous penetration tests against enterprise networks. Organizations often leave DTP enabled on access ports by default — a misconfiguration that allows attackers connected to a standard wall port to negotiate a trunk and access sensitive VLANs such as management or finance networks.

---

## 4. Tools Used by Attackers

| Tool | Description |
|------|-------------|
| **Yersinia** | Framework for attacking Layer 2 protocols including DTP and 802.1Q |
| **Scapy** | Craft custom double-tagged 802.1Q packets |
| **Wireshark** | Analyze VLAN tags in captured traffic |
| **DTPHijack** | Specialized DTP negotiation attack tool |

---

## 5. Detection Methods

| Method | Details |
|--------|---------|
| **Monitor for DTP packets** | Access ports should never send/receive DTP — flag any DTP traffic |
| **Port-based VLAN auditing** | Regularly review which ports are trunk vs. access |
| **IDS on switch traffic** | Alert on 802.1Q double-tagged packets |
| **SIEM correlation** | Detect unexpected cross-VLAN communication |
| **Switch log review** | Log trunk negotiation events and alert on unexpected trunk formation |

---

## 6. Defensive Measures

| Defense | Description |
|---------|-------------|
| **Disable DTP on all access ports** | `switchport nonegotiate` — prevents trunk auto-negotiation |
| **Explicitly set port modes** | Never leave ports in auto/desirable DTP mode |
| **Change native VLAN from VLAN 1** | Use an unused, dedicated VLAN as native VLAN |
| **Tag native VLAN traffic** | Explicitly tag all traffic including native VLAN |
| **VLAN access control lists (VACLs)** | Enforce strict access controls between VLANs |
| **Network audits** | Regularly audit switch configurations for misconfigurations |
| **Private VLANs (PVLANs)** | Further isolate hosts within the same VLAN |

### Cisco Hardening Commands:
```bash
# Disable DTP on access port
switchport mode access
switchport nonegotiate

# Change native VLAN
switchport trunk native vlan 999

# Explicitly allow only required VLANs on trunk
switchport trunk allowed vlan 10,20,30
```

---

## 7. MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| **Tactic** | Lateral Movement |
| **Technique** | T1599 – Network Boundary Bridging |
| **Sub-technique** | T1599.001 – Network Address Translation Traversal |
| **Link** | https://attack.mitre.org/techniques/T1599/ |

---

## 8. References

- MITRE ATT&CK T1599: https://attack.mitre.org/techniques/T1599/
- Cisco – VLAN Security Best Practices: https://www.cisco.com/c/en/us/support/docs/lan-switching/8021q/17056-741-4.html
- IEEE 802.1Q Standard
- Yersinia Tool Documentation: https://github.com/tomac/yersinia
