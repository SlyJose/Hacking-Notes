---
domain: red-team-fundamentals
tags: [emulation, simulation, threat-intelligence, engagement]
tools: []
opsec: n/a
exam-relevance: core
---

# Adversary Emulation vs Simulation

## Comparison

| | Emulation | Simulation |
|--|-----------|------------|
| **Threat** | Specific known actor | Hypothetical actor |
| **TTPs** | Known/documented | Unknown or unique |
| **Scope** | Narrow | Broad |
| **Outcome** | Tune defences against that specific threat | Expose broader blind spots |

## When to Use Each

- **Emulation** — use TI reports to mirror a real actor's TTPs; best when your client has known industry-specific threat actors
- **Simulation** — greater TTP freedom; better for finding unknown gaps

## Recommended Strategy

Emulation first to **set a baseline**, then simulation to **expand capability** against more advanced or varied TTPs.

## Related Notes

- [[Red-Teaming-Overview]]
- [[TTPs-Framework]]
- [[Engagement-Planning]]
