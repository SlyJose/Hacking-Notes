---
domain: red-team-fundamentals
tags: [kill-chain, attack-lifecycle, mandiant, lockheed-martin]
tools: []
opsec: n/a
exam-relevance: core
---

# Attack Lifecycle

## Cyber Kill Chain (Lockheed Martin — 7 phases)

1. Reconnaissance
2. Weaponisation
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Actions on Objectives

**Limitation:** Lacks detail on adversary behaviour post-initial-compromise.

## Mandiant Targeted Attack Lifecycle (8 phases)

1. **Initial Reconnaissance** — research target systems and employees
2. **Initial Compromise** — execute malicious code via planned attack vector
3. **Establish Foothold** — install persistent backdoors
4. **Escalate Privileges** — exploit misconfigurations for local admin
5. **Internal Reconnaissance** — explore internal infrastructure
6. **Move Laterally** — use obtained credentials to compromise additional systems
7. **Maintain Presence** — maintain privileged domain/system access
8. **Complete Mission** — accomplish operational objective

## Mapping to CRTO Domains

| Phase | Domain |
|-------|--------|
| 1–2 | [[02-Domains/Initial-Access/MOC\|Initial Access]] |
| 3 | [[02-Domains/Host-Persistence/MOC\|Host Persistence]] |
| 4 | [[02-Domains/Windows-Privilege-Escalation/MOC\|Windows Privilege Escalation]] |
| 5 | [[02-Domains/Domain-Recon/MOC\|Domain Recon]] + [[02-Domains/Host-Recon-Enumeration/MOC\|Host Recon]] |
| 6 | [[02-Domains/Lateral-Movement/MOC\|Lateral Movement]] |
| 7 | [[02-Domains/Domain-Dominance/MOC\|Domain Dominance]] |
| 8 | [[02-Domains/Data-Hunting-Exfiltration/MOC\|Data Hunting & Exfiltration]] |

## Related Notes

- [[TTPs-Framework]]
- [[Emulation-vs-Simulation]]
