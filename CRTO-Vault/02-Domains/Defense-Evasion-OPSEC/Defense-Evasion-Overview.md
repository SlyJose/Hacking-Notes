---
domain: defense-evasion-opsec
tags: [defense-evasion, TA0005, av-evasion, signatures, heuristics, overview]
tools: [Cobalt-Strike]
opsec: n/a
exam-relevance: core
---

# Defense Evasion Overview

**MITRE:** [TA0005](https://attack.mitre.org/tactics/TA0005/) — not a one-time step, but a continuous concern throughout an engagement.

## Two AV Detection Strategies

### 1. Signature-Based
Targets known, static portions of a payload. In the Cobalt Strike context:
- Raw Beacon DLL
- Prepended loader
- Payload artifacts (.exe, .dll, .ps1)

Triggers on disk drop or memory load — before or just after execution.

### 2. Behavioural Heuristics
Monitors post-exploitation actions once a payload is running:
- Spawning new processes
- Running shell commands
- Remote process injection
- Executing 3rd-party post-exploitation tools

## Goals for This Domain

| Tool / Technique | What it addresses |
|-----------------|-------------------|
| [[Artifact-Kit]] | Remove known indicators from compiled artifacts (.exe, .dll) |
| [[Resource-Kit]] | Remove known indicators from script artifacts (.ps1) |
| [[Malleable-C2]] | Modify how the prepended loader loads Beacon into memory |
| Malleable C2 | Modify post-exploitation behaviours |

## Key Caveat

AV evasion is a **moving target** — signatures and heuristics update constantly. The focus is on **methodology** (how to identify and implement bypasses), not on specific bypasses that may be patched tomorrow.

## Related Notes

- [[02-Domains/Malware-Essentials/C2-Payload-Architecture]]
- [[02-Domains/Malware-Essentials/Process-Injection-Overview]]
- [[02-Domains/C2-Infrastructure-Cobalt-Strike/Beacon-Payloads]]
