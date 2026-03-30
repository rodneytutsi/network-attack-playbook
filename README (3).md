# 📄 SYN Flood (DoS Attack)

| Field | Details |
|-------|---------|
| **Category** | Denial of Service |
| **MITRE Tactic** | Impact |
| **MITRE Technique** | T1499.002 – Service Exhaustion Flood |
| **Severity** | 🟠 Medium |
| **Protocols Affected** | TCP |

---

## 1. What Is It?

A SYN Flood is a **Denial of Service (DoS)** attack that exploits the **TCP three-way handshake**. The attacker sends a massive number of TCP SYN requests to a target server but never completes the handshake. This causes the server to keep connections half-open, exhausting its resources until it can no longer serve legitimate users.

---

## 2. How It Works

### Background: TCP Three-Way Handshake
Normal TCP connection setup:
```
Client  ──SYN──►  Server       (Client: "I want to connect")
Client  ◄──SYN-ACK──  Server   (Server: "OK, I'm ready")
Client  ──ACK──►  Server       (Client: "Great, let's go!")
```

### Attack Steps
```
Step 1: Attacker sends thousands of SYN packets to the target server,
        often with SPOOFED source IP addresses.

Step 2: Server responds with SYN-ACK to each request and waits for ACK.
        These are stored as "half-open connections" in memory.

Step 3: Attacker never sends the final ACK.

Step 4: Server's connection table fills up with half-open connections.

Step 5: Legitimate users cannot connect — server is unavailable.
```

### Diagram
```
Attacker (spoofed IPs)          Target Server
  SYN ──────────────────────►  [half-open connection saved]
  SYN ──────────────────────►  [half-open connection saved]
  SYN ──────────────────────►  [half-open connection saved]
  ...  (thousands per second)  [memory exhausted]
                                ❌ Legitimate users denied
```

---

## 3. Real-World Example

In 2018, GitHub experienced one of the largest DDoS attacks ever recorded at **1.35 Tbps**, partly utilizing SYN flood techniques combined with amplification. The attack lasted approximately 20 minutes before being mitigated by Akamai's DDoS protection service.

---

## 4. Tools Used by Attackers

| Tool | Description |
|------|-------------|
| **hping3** | Powerful packet crafting tool — can send high-rate SYN floods |
| **Scapy** | Python library for crafting custom TCP/SYN packets |
| **LOIC** | Low Orbit Ion Cannon — basic DoS tool |
| **Botnets** | Distributed SYN floods from thousands of compromised hosts |

---

## 5. Detection Methods

| Method | Details |
|--------|---------|
| **Netstat monitoring** | High number of `SYN_RECEIVED` connections is a red flag |
| **Traffic rate analysis** | Sudden spike in inbound SYN packets from many IPs |
| **IDS/IPS signatures** | Snort/Suricata rules to detect SYN flood patterns |
| **Firewall logs** | Watch for connection attempts that never complete |
| **NetFlow analysis** | Detect volumetric anomalies in traffic flow data |

### Netstat Detection Command:
```bash
netstat -an | grep SYN_RECV | wc -l
```
A very high count indicates a possible SYN flood.

---

## 6. Defensive Measures

| Defense | Description |
|---------|-------------|
| **SYN Cookies** | Server generates a cryptographic cookie instead of saving half-open state — memory is not consumed until ACK received |
| **Rate limiting** | Limit SYN packets per IP per second at the firewall |
| **Firewall rules** | Drop packets from IPs exceeding connection thresholds |
| **DDoS Protection Services** | Cloudflare, Akamai, AWS Shield absorb and filter flood traffic |
| **Increase backlog queue** | Temporarily increase the half-open connection limit |
| **Ingress filtering (BCP38)** | ISPs block spoofed source IPs at their borders |

---

## 7. MITRE ATT&CK Mapping

| Field | Value |
|-------|-------|
| **Tactic** | Impact |
| **Technique** | T1499 – Endpoint Denial of Service |
| **Sub-technique** | T1499.002 – Service Exhaustion Flood |
| **Link** | https://attack.mitre.org/techniques/T1499/002/ |

---

## 8. References

- MITRE ATT&CK T1499.002: https://attack.mitre.org/techniques/T1499/002/
- Cloudflare – What is a SYN Flood Attack?: https://www.cloudflare.com/learning/ddos/syn-flood-ddos-attack/
- RFC 4987 – TCP SYN Flooding Attacks and Common Mitigations
- GitHub Engineering Blog – February DDoS Incident (2018)
