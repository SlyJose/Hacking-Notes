

## 🖊️ Proxychains

`proxychains` is a tool which acts as a wrapper around other applications to tunnel their traffic over a socks proxy.


#### 📔 Proxychains with Cobalt Strike

1) First modify config file of proxychains and point it to the [[SOCKS Proxy]] you created in the C2 server:

`attacker@ubuntu ~> sudo vim /etc/proxychains.conf`

2) At the bottom change accordingly to your settings

	``socks4 127.0.0.1 1080``
	`socks5 127.0.0.1 1080 myUser myPassword`

	Depends if you are using auth or not, or if the tool you are planning to tunnel needs an specific socks version.

3) To tunnel a tool through proxychains, it's as simple as `proxychains [tool] [tool args]`.  

Check examples of app usage:

- [[Pivot with Nmap]]
- [[Pivot with WMI]]


##  📗 Action to perform 


### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 17:01
🏷️ tags: #redteam #crto 

---

