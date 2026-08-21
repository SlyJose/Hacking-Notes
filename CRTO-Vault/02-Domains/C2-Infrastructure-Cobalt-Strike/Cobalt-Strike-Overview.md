---
domain: c2-infrastructure-cobalt-strike
tags: [cobalt-strike, c2, beacon, team-server, overview]
tools: [Cobalt-Strike]
opsec: n/a
exam-relevance: core
---

# Cobalt Strike Overview

Post-exploitation C2 framework simulating stealthy, long-term embedded threat actors. Has built-in MITRE mapping, reporting, and TTPs for initial access through lateral movement.

## Components

| Component | Role |
|-----------|------|
| **Beacon** | The implant/agent — communicates with team server, fetches and executes jobs, returns results. Implemented as a Windows DLL. |
| **Team Server** | Central control and logging. Stores engagement data; drives reporting and Beacon workflows. |
| **Client** | Operator interface — connects to one or more team servers simultaneously, aggregates sessions, listeners, and data into a single view. |

## Distributed Operations

Design pattern: **one team server per engagement phase** (initial access, post-exploitation, persistence). Rationale: if one channel is burned, the others survive.

The CS client consolidates data across all connected team servers — listeners and active sessions appear as one unified view regardless of which server they originate from.

## Related Notes

- [[Beacon-Listeners]]
- [[Beacon-Payloads]]
- [[Interacting-with-Beacon]]
