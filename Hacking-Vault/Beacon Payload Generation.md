
Beacon itself is written as a reflective DLL based on Steven Fewer's [work](https://github.com/stephenfewer/ReflectiveDLLInjection) - so there's the core Beacon DLL (everything that make Beacon actually function) plus a reflective loader component.  

Those are then converted into position independent shellcode, which when injected and executed, calls the reflective loader entry point.  The reflective loader then loads the Beacon DLL into memory and kicks off a new thread to run it.

![[reflective-loader.png]]

Settings from your [[Malleable C2 Profile]] , such as the callback addresses are stomped into the DLL at the time your payloads are generated.  When you generate payload artifacts such as the EXE's, DLL's, and PowerShell, this Beacon shellcode is generated, XOR'd, and then stomped into them.  

The artifacts themselves act as "simple" shellcode injectors.  They all, with the exception of the service binary, inject Beacon shellcode into themselves.  That flow looks something like this:

![[artifact.png]]

The service binary is identical except that it spawns another process and performs remote injection instead.

**The complication when trying to bypass AV signatures is knowing which "part" of the payload they apply to - the core Beacon, the reflective loader, or the artifact.**








#### 🖊️ A


#### 📔 B


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 10:18
🏷️ tags: #redteam #crto 

---

