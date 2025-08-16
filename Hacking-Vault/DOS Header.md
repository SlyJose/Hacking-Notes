The DOS header is a fixed 64-byte structure, called `IMAGE_DOS_HEADER`, that exists at the start of every PE file.  Most of the members are not used anymore, but the two important ones are:

#### 🚀 -  e_magic

This is the first member and is a 2-byte WORD.  This value is always **4D 5A**, or **MZ** in ASCII.  It's simply a signature that marks the beginning of the PE.

---
#### 📦 - e_lfanew
--- 
This is the last member and is a 4-byte LONG.  It contains an offset to the PE signature at the start of the NT headers.  Because it's the last member, it is always located at an offset of **3C** (60 in decimal) from the start of the PE file.

![[DOS Header.png]]



### Properties
---
📆 created   {{16-08-2025}} 12:28
🏷️ tags: #redteam #crto 

---
