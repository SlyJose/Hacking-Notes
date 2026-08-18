---
domain: red-team-fundamentals
tags: [engagement, roe, tradecraft, planning, logging]
tools: []
opsec: n/a
exam-relevance: core
---

# Engagement Planning

## Goal Planning — Key Questions

- What ability does the threat have to gain physical/remote access?
- What ability does the threat have to gain elevated (local/domain admin) access?
- What ability does the threat have to move freely through the network?
- What ability does the threat have to identify and access sensitive data?
- What ability does the threat have to exfiltrate data?
- How long can the threat go undetected? What triggers a blue team response?
- What potential business impacts could the threat realise?

## Rules of Engagement (ROE)

Must cover:
- **Authorised targets** — domains, IP ranges, systems
- **Restrictions** — blacklisted targets or TTPs
- **Engagement objectives** — what success looks like

No engagement without an agreed ROE. Any deviations require approval from all parties.

## Tradecraft Do's

- **Log everything** — commands, timestamps, output, screenshots before/during/after key actions
- **Understand your tools** — know what artefacts, registry changes, and network traffic they produce
- **Situational awareness** — enumerate each new system: confirm in-scope, identify protections, check active sessions

## Tradecraft Don'ts

- **No untested tools** — vet all public/open-source tools before use on an engagement
- **No unencrypted C2** — always encrypt C2 traffic; cleartext credentials are sniffable
- **No real data exfiltration** — proof of access only; never pull PII, PCI, or HIPAA data
- **No disabling security controls** — no killing AV/firewalls without explicit client consent

## Related Notes

- [[Red-Teaming-Overview]]
- [[Emulation-vs-Simulation]]
