---
domain: red-team-fundamentals
tags: [ttps, tactics, techniques, procedures]
tools: []
opsec: n/a
exam-relevance: core
---

# TTPs Framework

## Hierarchy

| Level | The... | Definition | Example |
|-------|--------|-----------|---------|
| **Tactic** | *Why* | Overall tactical goal | Credential Access |
| **Technique** | *How* | Method to achieve the tactic | Dump LSASS memory |
| **Procedure** | *Exact steps* | Specific implementation | `sekurlsa::logonpasswords` via Mimikatz |

## Chaining

Tactics chain together — output of one feeds the next:

```
Privilege Escalation → Credential Access → Lateral Movement
```

Multiple techniques can achieve the same tactic. Multiple procedures can implement the same technique.

## Related Notes

- [[Attack-Lifecycle]]
- [[Emulation-vs-Simulation]]
