
Cobalt strike is actually bundled with multiple flavours of Mimikatz in both x86 and x64 builds.

```
PS C:\Tools\cobaltstrike\arsenal-kit\kits\mimikatz> ls

    Directory: C:\Tools\cobaltstrike\arsenal-kit\kits\mimikatz

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        05/12/2022     16:57           1046 build.sh
-a----        05/12/2022     16:57         773120 mimikatz-chrome.x64.dll
-a----        05/12/2022     16:57         638464 mimikatz-chrome.x86.dll
-a----        05/12/2022     16:57         813568 mimikatz-full.x64.dll
-a----        05/12/2022     16:57         704000 mimikatz-full.x86.dll
-a----        05/12/2022     16:57        1421824 mimikatz-max.x64.dll
-a----        05/12/2022     16:57        1192960 mimikatz-max.x86.dll
-a----        05/12/2022     16:57         312832 mimikatz-min.x64.dll
-a----        05/12/2022     16:57         276480 mimikatz-min.x86.dll
-a----        05/12/2022     16:57           2661 README.md
-a----        05/12/2022     16:57           1007 script_template.cna
```

**"full" version**: DLLs custom-built to include a reflective loader and modified code to achieve a smaller file size, which is required to work with Beacon's legacy 1 MB size limit.

**"max" version**: include the complete Mimikatz codebase, which can be used with CS 4.6 and above as the 1 MB limit can be removed.  

**"chrome" version**: contains code pertinent to Beacon's `chromedump` command.  



### Properties
---
📆 created   {{06-03-2024}} 14:46
🏷️ tags: #redteam #crto 

---

