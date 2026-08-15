
Payloads of different formats can be generated from the **Payloads** menu.  Here's a breakdown of each option:

### HTML Application

Produces a `.hta` file (typically delivered through a browser by way of social engineering) uses embedded VBScript to run the payload.  Only generates payloads for egress listeners and is limited to x86.  
  
### MS Office Macro

Produces a piece of VBA that can be dropped into a macro-enabled MS Word or Excel document.  Only generates payloads for egress listeners but is compatible with both x86 and x64 Office.

### Stager Payload Generator

Produces a payload stager in a variety of languages including C, C#, PowerShell, Python, and VBA.  These are useful when building your own custom payloads or exploits.  Only generates payloads for egress listeners, but supports x86 and x64.

### Stageless Payload Generator

As above, but generates stageless payloads rather than stagers.  It has slightly fewer output formats, e.g. no PowerShell, but has the added option of specifying an exit function (process or thread).  It can also generate payloads for P2P listeners.

### Windows Stager Payload

Produces a pre-compiled stager as an EXE, Service EXE or DLL.

### Windows Stageless Payload

Produces a pre-compiled stageless payload as an EXE, Service EXE, DLL, shellcode, as well as PowerShell.  This is also the only means of generating payloads for P2P listeners.

### Windows Stageless Generate All Payloads

Pretty much what it says on the tin.  Produces every stageless payload variant, for every listener, in x86 and x64.



### Properties
---
📆 created   {{06-02-2024}} 10:26
🏷️ tags: #redteam #crto  

---

