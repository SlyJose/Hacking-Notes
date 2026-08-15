---
domain: defense-evasion-opsec
tags: [moc]
---

# Defense Evasion & OPSEC — Map of Content

## Techniques

- [[AMSI-Bypass]]

## Detection & OPSEC Notes

This is the meta-domain — every technique note in every other domain should consider OPSEC. This MOC consolidates evasion-specific techniques and cross-references OPSEC notes from other domains.

### General Principles

- Prefer in-memory execution over dropping to disk
- Use `execute-assembly` over PowerShell when possible
- Match your C2 traffic to expected network patterns
- Kill sleep-0 Beacons when not actively using them
- Clean up artifacts (services, scheduled tasks, dropped files) when done

## Related Tools

- Cobalt Strike (`argue`, `blockdlls`, `ppid`, `spawnto`)
- Custom loaders / packers

## Open Questions

- 
