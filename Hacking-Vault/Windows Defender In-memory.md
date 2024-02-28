

#### 📔 - In memory

Malicious processes can also trigger alerts during memory scans, Defender will also carry process scans to determine weird patterns in form and actions. 

###### 🚀 - Cobalt Strike
---
1. Beacon is also susceptible to being caught whilst it's running in memory or on a process scan. 
2. Your artifact payload may be fine (no signature detections) but your raw shellcode could be the one causing a detection. The problem is beacon source code is not public. 
3. To handle this problem we use [[Malleable C2 Profile]], which can mold the actions of the beacon in memory. 

--- 






### Properties
---
📆 created   {{28-02-2024}} 15:16
🏷️ tags: #redteam #crto 

---

