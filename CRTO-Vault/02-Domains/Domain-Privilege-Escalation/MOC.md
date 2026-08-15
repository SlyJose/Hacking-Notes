---
domain: domain-privilege-escalation
tags: [moc]
---

# Domain Privilege Escalation — Map of Content

## Techniques

- [[Unconstrained-Delegation]]

## Detection & OPSEC Notes

- Delegation attacks manipulate Kerberos — monitor for unusual TGT forwarding
- ACL abuse is harder to detect but leaves audit trail if SACL is configured
- BloodHound is the best tool for discovering these paths before exploiting them

## Related Tools

- Rubeus (delegation, S4U)
- StandIn (ACL abuse)
- PowerView (ACL enumeration)
- BloodHound (attack path discovery)

## Open Questions

- 
