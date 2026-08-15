---
domain: host-recon-enumeration
tags: [moc]
---

# Host Recon & Enumeration — Map of Content

## Techniques

- [[Seatbelt-Enumeration]]

## Detection & OPSEC Notes

- Most enumeration is low-noise (reading local config, listing processes)
- Running .NET assemblies via `execute-assembly` leaves fewer artifacts than PowerShell
- Avoid `net` commands that generate 4688 events when possible

## Related Tools

- Seatbelt
- SharpUp
- Beacon built-in commands (`ps`, `net`, `shell whoami`)

## Open Questions

- 
