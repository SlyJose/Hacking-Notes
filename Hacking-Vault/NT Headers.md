The Windows SDK (software development kit) has two definitions for the NT header - `IMAGE_NT_HEADERS` for 32-bit PEs and `IMAGE_NT_HEADERS64` for 64-bit PEs.

---
#### 🚀 - COFF Header
---
The COFF header composes of PE Signature and the File Header.

 1️⃣ PE Signature

Like the e_magic member of the DOS header, the PE signature is a 4-byte DWORD that always has a fixed value of **50 45 00 00** which is **PE\0\0** in ASCII.  This is used to verify that the start of the NT header has been correctly located. (Revisit the [[PE File Structure]] image to find the signature location, it's in the green section)

 2️⃣ File header

 
![[File Header.png]]

The file header is a structure called `IMAGE_FILE_HEADER`, which has 7 members.  Some key ones are:

- **Machine** - a 2-byte WORD that indicates the CPU architecture the PE is compiled for.  The possible values are documented [here](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format#machine-types).
    
- **NumberOfSections** - a WORD that holds the number [[PE Sections]] the PE has.
    
- **SizeOfOptionalHeader** - a 2-byte WORD that holds the size of the optional header.
    
- **Characteristics** - a 2-byte flag that describes some [attributes](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format#characteristics) of the PE.

---
#### 🖊️ - Optional Header
---
![[Optional Header.png]]
The optional header structure can either be `IMAGE_OPTIONAL_HEADER32` or `IMAGE_OPTIONAL_HEADER64` depending on the PE architecture.  Some key members include:

- **Magic** - a value that determines whether the image is PE32 (i.e. 32-bit) or PE32+ (i.e. 64-bit).
    
- **AddressOfEntryPoint** - the address of the PE's entry point relative to the image base when loaded into memory.
    
- **ImageBase** - the preferred base address for the PE image to be loaded into.
    
- **NumberOfRvaAndSizes** - the size of the _DataDirectory_ array.
    
- **DataDirectory** - an array of `IMAGE_DATA_DIRECTORY` structures.

❗Data Directories

Also a part of the optional headers, the `IMAGE_DATA_DIRECTORY` structure contains two members.  A _VirtualAddress_, which points to the start of a particular data directory structure; and a _Size_, which is the size of that data directory.  These data directories contain information needed by the Windows loader, for example, the import directory contains details of the modules (DLLs) that that PE requires to function.

### Properties
---
📆 created   {{16-08-2025}} 12:45
🏷️ tags: #redteam #crto 

---
