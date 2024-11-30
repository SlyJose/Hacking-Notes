
One major challenge in relaying Kerberos is that service tickets are encrypted with the service's secret key.  A ticket for CIFS/HOST-A cannot be relayed to CIFS/HOST-B because HOST-B would be unable to decrypt a ticket that was encrypted for HOST-A.  

However, in Windows, the service's secret key is derived from the principal associated with its SPN and is not necessarily unique per-service.  Most services run as the local SYSTEM, which in a domain context, is the computer account in Active Directory.  Therefore, service tickets for services run on the same host, such as CIFS/HOST-A and HTTP/HOST-A, would be encrypted with the same key.

## 🖊️ How it works

The attacker starts a malicious RPC server that will force connecting clients to authenticate to it using Kerberos only, and by using appropriate security bindings, they can specify a completely arbitrary SPN.  This will force a service ticket to be generated for a service/SPN that that attacker doesn't control, such as HOST/DC.  They then coerce a privileged COM server into connecting to their malicious RPC server, which will perform the authentication and generate the appropriate Kerberos tickets.  

As an example, the malicious RPC server would receive a KRB_AP_REQ for HOST/DC as the local computer account, which the attacker can relay to LDAP/DC instead.  With a valid service ticket for LDAP, they can submit requests to the DC as the computer account to modify the computer object in Active Directory.  

This opens the door for other attacker primitives like RBCD and shadow credentials in order to achieve the LPE.

Some tools that allow this attack are:

[KrbRelayUp](https://github.com/ShorSec/KrbRelayUp) (automated)
[KrbRelay](https://github.com/cube0x0/KrbRelay) (more manual)

#### Exploitation

- [[Run Kerberos Relay Attack]]



### ⚠ Opsec




### Properties
---
📆 created   {{19-02-2024}} 14:52
🏷️ tags: #redteam #crto 

---

