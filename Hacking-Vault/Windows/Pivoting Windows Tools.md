
We can also pivot using an attacker Windows machine.

## 🖊️ Proxifier

1) To create a new proxy entry, go to **Profile > Proxy Servers**.  Click **Add** and enter the relevant details.
2) To create proxy rules you can follow this example:

```
Click **Add** to create a new rule and use the following:

- Name:  Tools
- Applications:  Any
- Target hosts:  10.10.120.0/24;10.10.122.0/23
- Target ports:  Any
- Action:  Proxy SOCKS5 10.10.5.50
```

3) To enable authentication to occur over the proxy, an application needs to be launched as a user from the target domain.  This can be achieved using `runas /netonly` or Mimikatz.

You can use Proxifier with tools like these to operate further into the environment:

- [[Pivot with mmc.exe]]
- [[Pivot with Powershell]]


##  📗 Action to perform 


### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 17:14
🏷️ tags: #redteam #crto 

---

