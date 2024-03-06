
The ".cna" files that we load into the Cobalt Strike Script Manager are called **Aggressor Scripts**. These can override default behaviours in Cobalt Strike to customise the UI (add new menus, commands, etc), extended the data models, extended existing commands like `jump`, and add brand new, custom commands. 

Beacon also has an internal API that we can call from Aggressor, so any base primitive that Beacon has (`powershell`, `execute-assembly`, etc) can be called from Aggressor.

The Aggressor script reference is public and available at [helpsystems.com](https://hstechdocs.helpsystems.com/manuals/cobaltstrike/current/userguide/content/topics/agressor_script.htm). The underlying programming language used is called [Sleep](http://sleep.dashnine.org/manual/index.html).


#### 📔 Aggressor Script Samples

- [[Jump + Invoke-DCOM]]

### Properties
---
📆 created   {{04-03-2024}} 09:46
🏷️ tags: #redteam #crto 

---

