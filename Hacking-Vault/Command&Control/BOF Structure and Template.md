
BOFs are essentially tiny [COFF](https://en.wikipedia.org/wiki/COFF) objects for which Beacon acts as a linker and loader.  Beacon does not link BOFs to a standard C library, so many functions that you may be used to are not available.  

Though it does expose several internal APIs that can be utilized to simplify some actions, such as argument parsing and sending output.

#### 🖊️ BOF Template

The easiest way to get started with writing a BOF is with the official Visual Studio project template.

![[vs-template.png]]

The `bof.cpp` file contains some boilerplate code that demonstrates various features of the project template, most notably:

- The `DFR` and `DFR_LOCAL` macros for Dynamic Function Resolution.
- The main function for debug builds and the ability to provide mock packed arguments.
- The new unit testing functionality.



#### ⚠ Opsec




### Properties
---
📆 created   {{06-03-2024}} 15:46
🏷️ tags: #redteam #crto 

---

