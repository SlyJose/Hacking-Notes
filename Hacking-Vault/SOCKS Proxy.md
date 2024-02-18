
A SOCKS (short for Socket Secure) Proxy exchanges network packets between a client and a server. 

Proxies take incoming traffic, filter it, send it to the destination, and do the same with incoming traffic. 

## 🖊️ via [[Cobalt Strike]]

We can use this idea in an offensive application by turning our C2 server into a SOCKS proxy to tunnel external tooling into an internal network. Cobalt uses SOCKS4a and SOCKS5.

The main difference between them is that only SOCKS5 supports authentication and has some additional logging capabilities.


#### 📔 Usage 

1) to start the socks proxy:

SOCKS4a:   `beacon > socks 1080`
SOCKS5:     `beacon > socks 1080 socks5 disableNoAuth myUser myPassword enableLogging `

You can also start them in your SOCKS proxy section of the GUI in Cobalt Strike.


### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 16:52
🏷️ tags: #redteam #crto 

---

