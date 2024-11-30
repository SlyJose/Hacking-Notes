
A Windows "service" is a special type of application that is usually started automatically when the computer boots. Example services: Windows functionality such as Windows Defender, Windows Firewall, Windows Update.

Third party applications may also install a Windows Service to manage how and when they're run.

## 🖊️ Enumeration

1) Do enumeration of the services with commands like:

`sc query` or `Get-Service`

2) Important properties to pay attention at:

- Binary Path: locatin of the .exe
- Startup type: when is the service starting?
- Service status: running, stopped, pending
- Log On as: The user account that the service is configured to run as.This could be a domain or local account. It's very common for these services to be run as highly-privileged accounts, even domain admins, or as local system. This is why services can be an attractive target for both local and domain privilege escalation.
- Dependants & Dependencies: These are services that either the current service is dependant on to run, or other services that are dependant on this service to run. This information is mainly important to understand the potential impact of manipulation. Like files and folders - services themselves (not just the .exe) have permissions assigned to them. This controls which users can modify, start or stop the service. Some highly sensitive services such as Windows Defender cannot be stopped, even by administrators. Other services may have much weaker permissions that allow standard users to modify them for privilege escalation.

## 📔 Exploitation Methods

- [[Unquoted Service Paths]]
- [[Weak Service Permissions]]
- [[Weak Service Binary Permissions]]
- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{10-02-2024}} 10:56
🏷️ tags: #redteam #crto 

---

