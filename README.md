# 🌐 Network Attack Playbook

> A structured reference guide documenting real-world network attacks, how they work, how to detect them, and how to defend against them.

---

## 👤 About

This playbook is part of my cybersecurity master's portfolio. It demonstrates my understanding of common network-based attacks from both an **offensive (attacker)** and **defensive (defender)** perspective, mapped to the **MITRE ATT&CK framework**.

---

## 📁 Repository Structure

```
network-attack-playbook/
│
├── README.md                        ← You are here
├── attacks/
│   ├── arp-spoofing/
│   │   └── README.md
│   ├── dns-poisoning/
│   │   └── README.md
│   ├── syn-flood/
│   │   └── README.md
│   ├── man-in-the-middle/
│   │   └── README.md
│   ├── port-scanning/
│   │   └── README.md
│   ├── credential-sniffing/
│   │   └── README.md
│   └── vlan-hopping/
│       └── README.md
│
├── methodology/
│   └── research-methodology.md      ← How each attack was researched
│
└── resources/
    └── references.md                ← Tools, frameworks, and learning resources
```

---

## 📊 Attacks Index

| # | Attack | Category | MITRE Tactic | Severity |
|---|--------|----------|--------------|----------|
| 1 | [ARP Spoofing](./attacks/arp-spoofing/README.md) | LAN / MitM | Collection | 🔴 High |
| 2 | [DNS Poisoning](./attacks/dns-poisoning/README.md) | Protocol Abuse | Impact | 🔴 High |
| 3 | [SYN Flood](./attacks/syn-flood/README.md) | Denial of Service | Impact | 🟠 Medium |
| 4 | [Man-in-the-Middle](./attacks/man-in-the-middle/README.md) | Interception | Collection | 🔴 High |
| 5 | [Port Scanning](./attacks/port-scanning/README.md) | Reconnaissance | Discovery | 🟡 Low |
| 6 | [Credential Sniffing](./attacks/credential-sniffing/README.md) | Passive Attack | Credential Access | 🔴 High |
| 7 | [VLAN Hopping](./attacks/vlan-hopping/README.md) | Network Evasion | Lateral Movement | 🟠 Medium |

---

## 📋 Each Attack Document Covers

- ✅ **What it is** — Simple explanation
- ✅ **How it works** — Step-by-step technical breakdown
- ✅ **Real-world example** — Known incident or use case
- ✅ **Tools used by attackers** — Common offensive tools
- ✅ **Detection methods** — How to spot it
- ✅ **Defensive measures** — How to stop it
- ✅ **MITRE ATT&CK mapping** — Tactic & technique IDs

---

## ⚠️ Disclaimer

This playbook is created **strictly for educational and research purposes**. All information is sourced from publicly available academic, vendor, and security research publications. No attacks were performed on any real or unauthorized systems.

---

## 📬 Contact

- 🔗 LinkedIn: *[Rodney Mavhunduke]*
- 📧 Email: *[rodneytutsi@gmail.com]*
