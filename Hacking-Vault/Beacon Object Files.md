Beacon Object Files (BOFs) are a post-ex capability that allows for code execution inside the Beacon host process.  The main advantage is to avoid the fork & run pattern that commands such as `powershell`, `powerpick` and `execute-assembly` rely on.  

Since these spawn a sacrificial process and use process injection to run the post-ex action, they are heavily scrutinised by AV and EDR products.  The downside is that because BOFs run inside the Beacon process, an unstable BOF may crash your Beacon.  Run with care or in a Beacon you don't mind losing.

#### 📗 - Structure

-  [[BOF Structure and Template]]
- [[BOF Arguments]]
- [[BOF Calling Windows APIs]]

---
#### 📦 - Integration

- [[Integrate BOF with Aggressor Scripts]]

--- 
#### 🖊️ Samples

- [[BOF Hello World]] 
- [[BOF Hello World Integrated]]


#### 📔 Useful Projects

- [CS-Situational-Awareness-BOF](https://github.com/trustedsec/CS-Situational-Awareness-BOF) by [TrustedSec](https://twitter.com/TrustedSec).
- [BOF.NET](https://github.com/CCob/BOF.NET) by [@_EthicalChaos_](https://twitter.com/_EthicalChaos_).
- [NanoDump](https://github.com/fortra/nanodump) by [Fortra](https://twitter.com/HelpSystemsMN).
- [InlineWhispers](https://github.com/outflanknl/InlineWhispers) by [Outflank](https://twitter.com/outflanknl).


### Properties
---
📆 created   {{06-03-2024}} 15:35
🏷️ tags: #redteam #crto 

---
