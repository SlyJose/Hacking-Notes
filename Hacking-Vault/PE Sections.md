
![[PE Sections.png]]
The PE sections contain the actual data and executable code of the program.  These typically include:

- **.text** - contains the executable code of the program.
    
- **.data** - contains initialised data. 
    
- **.bss** - contains uninitialised data.
    
- **.rdata** - contains read-only data.
    
- **.rsrc** - contains resources used by the program such as icons, images, etc.
    

Each section is appended to the a header that describes it.  A section header contains a:

- **Name** - the name of the section.  This field is limited to 8-bytes.
    
- **VirtualSize** - the total size of the section when loaded into memory.
    
- **VirtualAddress** - the memory address of the section when loaded into memory.  The value is an offset relative to the image's base address.
    
- **SizeOfRawData** - the size of the section as it is stored on disk.  This size may be different to its virtual size, e.g. if it's padded.
    
- **Characteristics** - a set of flags that describe the characteristics of the section.  One such characteristic is the final memory permissions that this section memory should be (e.g. R, RW, RX, etc).




### Properties
---
📆 created   {{16-08-2025}} 13:07
🏷️ tags: #redteam #crto 

---

