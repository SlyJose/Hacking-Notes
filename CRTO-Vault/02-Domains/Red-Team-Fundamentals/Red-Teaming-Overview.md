---
domain: red-team-fundamentals
tags: [red-team, definition, blue-team, opsec, threat-intelligence]
tools: []
opsec: n/a
exam-relevance: core
---

# Red Teaming Overview

## Definition

> "Red Teaming is the process of using TTPs to emulate a real-world threat, with the goal of measuring the effectiveness of the people, processes and technologies used to defend an environment."
> — Joe Vest & James Tubberville

## Goals

- Achieve a **pre-agreed operational objective** — e.g. access to a specific system or dataset
- Not generic goals like "get domain admin" — DA may be a stepping stone, not the objective
- Demonstrate **business risk** through impact of compromise

## Blue Teams

- Detect and respond using incident management processes
- Rely on log telemetry from workstations, servers, firewalls
- May or may not be notified that an assessment is underway

## OPSEC

Measures how likely red team actions can be observed and interrupted. Critical during simulation; less constrained during emulation. See [[02-Domains/Defense-Evasion-OPSEC/MOC]].

## Threat Intelligence (TI)

Used to frame red team scenarios, identify emerging threats, and compare activity against known TTPs/IOCs.

Common TI standards: CAPEC, CybOX, STIX, TAXII, Microsoft Interflow

## Primum Non Nocere ("First, do no harm")

Do not increase client risk unnecessarily:
- No disabling AV, firewalls, or other security controls
- No exfiltrating real sensitive data (PII, PCI, HIPAA) — proof of access only
- No backdoors that could be abused by third parties
- Agree on all boundaries before the engagement starts

## Related Notes

- [[TTPs-Framework]]
- [[Emulation-vs-Simulation]]
- [[Engagement-Planning]]
