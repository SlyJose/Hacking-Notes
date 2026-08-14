---
domain: domain-dominance
tags: [moc]
---

# Domain Dominance — Map of Content

## Techniques

- [[DCSync]]

## Detection & OPSEC Notes

- DCSync triggers Directory Replication Service events — well-detected
- Golden Tickets are hard to detect after creation but anomalous TGTs (wrong encryption, wrong domain SID) can be flagged
- Diamond/Sapphire Tickets are stealthier alternatives to Golden Tickets
- These techniques represent "game over" — use them to complete objectives, then document

## Related Tools

- Mimikatz (`lsadump::dcsync`, `kerberos::golden`)
- Rubeus (`golden`, `diamond`, `silver`)
- Beacon (`dcsync`)

## Open Questions

- 
