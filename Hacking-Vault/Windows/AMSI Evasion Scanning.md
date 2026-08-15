
#### 🖊️ via ThreatCheck

1) Scan your resource adding the `amsi` parameter and see the outcome of threatcheck 

```
PS C:\Users\Attacker> C:\Tools\ThreatCheck\ThreatCheck\bin\Debug\ThreatCheck.exe -f C:\Payloads\smb_x64.ps1 -e amsi
```

  ⚠ Ensure real-time protection is **enabled** in Defender before running ThreatCheck against script artifacts.

2) Once scanned, threatcheck will give you the byte offset where the signature is identifying the malicious code. Grab that value. 



### Properties
---
📆 created   {{28-02-2024}} 19:30
🏷️ tags: #redteam #crto 

---

