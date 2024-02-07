
Some organisations (particularly those with an internal PKI) will perform SSL offloading on HTTPS web traffic.  This allows the proxy to decrypt incoming HTTPS traffic and inspect the plaintext HTTP.  The traffic is then re-encrypted with an internally trusted private key before being forwarded to the client.

This means that even your HTTPS C2 traffic can be inspected.  Some C2 tools (such as [Covenant](https://github.com/cobbr/Covenant)) allow you to configure certificate pinning on the implants which would effectively prevent this from taking place, but at the potential cost of the proxy blocking the traffic entirely.

## 🖊️ Sub-Topic


## 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 13:47
🏷️ tags: #defense  
---

