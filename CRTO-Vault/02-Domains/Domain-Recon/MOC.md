---
domain: domain-recon
tags: [moc]
---

# Domain Recon — Map of Content

## Techniques

- [[SharpHound-Collection]]

## Detection & OPSEC Notes

- SharpHound generates significant LDAP traffic — consider using targeted collection methods
- `--CollectionMethod Session` queries remote machines and is noisier than `--CollectionMethod DCOnly`
- PowerView cmdlets may trigger AMSI if running in PowerShell

## Related Tools

- SharpHound (BloodHound collector)
- BloodHound (graph analysis)
- PowerView / SharpView
- ADSearch
- Beacon built-in (`net` commands)

## Open Questions

- 
