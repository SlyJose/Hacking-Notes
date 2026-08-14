---
domain: forest-trust-abuse
tags: [moc]
---

# Forest & Trust Abuse — Map of Content

## Techniques

- [[SID-History-Abuse]]

## Detection & OPSEC Notes

- Cross-domain ticket requests are logged on both DCs
- SID History modifications are rare and highly suspicious (Event ID 4765, 4766)
- SID filtering between forests may block SID History attacks

## Related Tools

- Mimikatz (`kerberos::golden` with `/sids`)
- Rubeus
- PowerView (`Get-DomainTrust`)
- BloodHound (cross-domain paths)

## Open Questions

- 
