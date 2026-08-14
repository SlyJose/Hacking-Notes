---
domain: lateral-movement
tags: [moc]
---

# Lateral Movement — Map of Content

## Techniques

- [[PsExec-Lateral-Movement]]

## Detection & OPSEC Notes

- Lateral movement is a high-detection-risk phase — defenders watch for it
- PsExec creates a service on the remote host (Event ID 7045)
- WinRM is quieter but requires port 5985/5986 open
- Always clean up services and artifacts after moving

## Related Tools

- Cobalt Strike (`jump`, `remote-exec`)
- Mimikatz (for PTH)
- Rubeus (for PTT)

## Open Questions

- 
