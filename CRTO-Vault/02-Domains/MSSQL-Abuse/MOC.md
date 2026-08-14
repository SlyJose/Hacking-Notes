---
domain: mssql-abuse
tags: [moc]
---

# MSSQL Abuse — Map of Content

## Techniques

- [[MSSQL-Linked-Servers]]

## Detection & OPSEC Notes

- xp_cmdshell execution is heavily logged and monitored
- Linked server queries can chain across multiple SQL instances
- SQL Server service account context determines OS-level access

## Related Tools

- SQLRecon
- PowerUpSQL
- Beacon (executing via MSSQL)

## Open Questions

- 
