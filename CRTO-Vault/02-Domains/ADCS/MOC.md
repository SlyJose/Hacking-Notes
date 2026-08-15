---
domain: adcs
tags: [moc]
---

# ADCS — Map of Content

## Techniques

- [[ESC1-Misconfigured-Template]]

## Detection & OPSEC Notes

- Certificate requests are logged — Event ID 4886 (certificate request), 4887 (certificate issued)
- Requesting a certificate as another user (SAN) is highly anomalous
- ADCS abuse is a newer attack surface — some orgs don't monitor for it yet

## Related Tools

- Certify (enumerate and request certificates)
- ForgeCert (forge certificates with stolen CA key)
- Rubeus (use certificates for Kerberos auth)

## Open Questions

- 
