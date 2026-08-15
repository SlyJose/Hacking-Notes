---
domain: host-persistence
tags: [moc]
---

# Host Persistence — Map of Content

## Techniques

- [[Scheduled-Task-Persistence]]

## Detection & OPSEC Notes

- Persistence is inherently high-risk — it creates artifacts that survive reboots
- Registry and scheduled task changes generate event logs
- Trade-off: reliability of callback vs. detection surface

## Related Tools

- Cobalt Strike Beacon (built-in persistence via `argue`)
- SharPersist

## Open Questions

- 
