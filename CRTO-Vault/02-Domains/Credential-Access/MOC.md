---
domain: credential-access
tags: [moc]
---

# Credential Access — Map of Content

## Techniques

- [[Kerberoasting]]

## Detection & OPSEC Notes

- LSASS access is heavily monitored by EDR — consider alternatives
- Kerberoasting generates 4769 events with RC4 encryption type
- DPAPI operations are lower noise than direct LSASS dumping
- Credential harvesting is a core exam skill — know multiple approaches

## Related Tools

- Rubeus (Kerberos attacks)
- Mimikatz (credential dumping)
- SharpDPAPI
- Beacon built-in (`hashdump`, `logonpasswords`, `dcsync`)

## Open Questions

- 
