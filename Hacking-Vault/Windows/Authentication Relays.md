
These attacks attempt to intercept either [[NetNTLM]] challenges or hashes for later on crack them and authenticate in the network.

The attack poisons challenges and wins a race condition to be the first to authenticate instead of the real machine. Responder will try to catch Responses in the authentication process, with that hash we will later on try to obtain the NTLM password associated with it by cracking it.  

The attack can be done by using a man-in-the-middle scenario, with a sniffing tool such as [[Responder]] .


## 📔 Catching the Challenge

1) Start Responder

`sudo responder -I [network interface]`

2) The tool starts listening for [[LLMNR]] and [[NetBios Name Service (NBNS)]]
3) Leave it running until an authentication attempt is catched
4) Once a Response is catched, grab and save the NTLM hash in a file
5) Crack the password hash

``hashcat -m 5600 <hash file> <password file> --force

If your dictionary is successful you will get the password.

## 📗 Relaying the Challenge

Another method is not catching the challenge but instead relaying it. 

The attacker:

1) Catches the ntlm negotiate
2) Sends it to the real server
3) Catches the ntlm challenge
4) Relays it to the client
5) Catches the client's Response
6) Relays it to the server successfully authenticating
7) Sends an error message to the client saying their auth (now stolen) failed

You need: 
- [[SMB]] Signing should be disabled
- Catched account must have admin privs on a server (we dont know which one). This is why this is a good method for lateral movement instead of establishing a foothold on [[Active Directory]], since if you enumerate first then relaying the challenge would be easier to know which one works and which one doesn't. 

Attack methods:

https://warroom.rsmus.com/how-to-perform-ntlm-relay/

### Properties
---
📆 created   {{27-11-2023}} 18:58
🏷️ tags: #activedirectory #lateralmovement

---

