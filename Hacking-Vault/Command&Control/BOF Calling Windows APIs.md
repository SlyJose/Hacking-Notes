
If we would manually wanted to call Windows APIs, the calls would look something like this:

`DECLSPEC_IMPORT INT WINAPI USER32$MessageBoxA(HWND, LPCSTR, LPCSTR, UINT);`

Which is strange and confussing.

BOF provides us with a simpler way, using `DFR - Dynamic Function Resolution`

#### 🖊️ DFR

Instead of that long command we can use something like:

`DFR(USER32, MessageBoxA);`
OR
`DFR_LOCAL(USER32, MessageBoxA);`

In this example, we are doing the same thing, calling messagebox from user32.dll

**DFR** is slightly longer, but they can be declared once and re-used in multiple functions.
**DFR_LOCAL** is shorter but they must be declared and used inside a function.

Check [[BOF Hello World Integrated]] for a full example.

#### ⚠ Opsec




### Properties
---
📆 created   {{06-03-2024}} 16:32
🏷️ tags: #redteam #crto 

---

