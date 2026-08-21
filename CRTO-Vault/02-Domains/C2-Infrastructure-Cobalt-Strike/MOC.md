---
domain: c2-infrastructure-cobalt-strike
tags: [moc]
---

# C2 Infrastructure & Cobalt Strike — Map of Content

## Techniques

- [[Cobalt-Strike-Overview]]
- [[Beacon-Listeners]]
- [[Beacon-Payloads]]
- [[Interacting-with-Beacon]]

## Detection & OPSEC Notes

- C2 traffic patterns are a primary detection vector — use Malleable C2 profiles
- Default Cobalt Strike indicators are well-known — always customize
- Sleep and jitter settings matter: low sleep = more traffic = more risk
- SMB and TCP Beacons don't leave the network — chain from an HTTP/HTTPS egress Beacon

## Related Tools

- Cobalt Strike (obviously)
- Redirectors (Apache, Nginx, CDNs)

## Open Questions

- 
