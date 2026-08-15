
The code for the entry point of each artifact format (i.e. EXE and DLL) can be found in `src-main`.

#### 🖊️ Artifacts

These files just call a function called `start`, so you should not need to modify these files in most cases, they are the basic of each artifact creation:

`dllmain.c` for the DLL artifacts
`main.c` for the EXE artifacts
`svcmain.c` for the Service EXE artifacts

The `start` function is implemented in each bypass file, the can be found in `src-common` and named as `bypass-<technique>.c`

#### 📔 Bypass Techniques

In each `bypass-<technique>.c`.  The included bypass methods are:

- mailslot - reads the shellcode over a mailslot.
- peek - uses a combination of Sleep, PeekMessage and GetTickCount.
- pipe - reads the shellcode over a named pipe.
- readfile - artifact reads itself from disk and seeks to find the embedded shellcode.


#### ⚠ Opsec




### Properties
---
📆 created   {{28-02-2024}} 10:46
🏷️ tags: #redteam #crto 

---

