---
domain: windows-privilege-escalation
tags: [moc]
---

# Windows Privilege Escalation — Map of Content

## Techniques

- [[Unquoted-Service-Paths]]

## Detection & OPSEC Notes

- Privilege escalation often involves writing files or modifying services — both leave artifacts
- UAC bypass techniques vary in stealth; some trigger Defender
- Token impersonation is relatively quiet if you already have SeImpersonatePrivilege

## Related Tools

- SharpUp (checks for priv esc vectors)
- Seatbelt
- Beacon built-in (`elevate`, `getuid`, `getsystem`)

## Open Questions

- 
