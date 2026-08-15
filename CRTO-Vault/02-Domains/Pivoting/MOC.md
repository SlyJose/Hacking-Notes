---
domain: pivoting
tags: [moc]
---

# Pivoting — Map of Content

## Techniques

- [[SOCKS-Proxy-Pivoting]]

## Detection & OPSEC Notes

- SOCKS proxy traffic goes through the Beacon — increases its bandwidth usage
- Reverse port forwards open a listening port on the team server
- SMB Beacon chaining is quieter but depends on SMB access

## Related Tools

- Cobalt Strike (`socks`, `rportfwd`, SMB/TCP listeners)
- Proxychains (Linux-side for routing tools through SOCKS)

## Open Questions

- 
