
####  📗 How a process is loaded into memory

Let's take a file "example.exe" on disk to explain this.

When you double-click `example.exe`, it's the **operating system's loader** that reserves the memory. The OS, through its kernel, creates a new process for `example.exe`. It then allocates a **virtual address space** for this new process.

Virtual memory is used by the operating system to map the process' memory to Ram or Disk. It does not contain code, data or instructions from the program.

Every process has its own private virtual memory that it uses to store data during its runtime.  The Windows memory manager transparently maps a process's virtual memory to physical memory, and may even page data to disk when needed.

![[Process Memory.png]]

**Why having virtual memory and not loading the whole program into memory?**

The OS does **not** load the entire program into RAM at once. This is the core reason for using pages and virtual memory. Modern programs can be hundreds of megabytes or even gigabytes in size. It would be highly inefficient to load all of that data into physical memory, especially if the user is running multiple programs simultaneously.

Instead, the OS only loads the necessary parts of the program into physical RAM as they are needed. This is known as **demand paging**. When you first double-click `example.exe`, the OS only loads the program's initial code and data pages. As the program executes and tries to access data or code that is not currently in physical memory, it triggers a **page fault**. The OS's page fault handler then loads that required page from the disk (from the original `example.exe` file or the paging file) into an available physical memory frame. This system allows the computer to run more and larger programs than would fit in RAM.

**What does the virtual memory contain?**

Virtual memory is a **map**, not a container of instructions. It's the OS's way of translating the logical addresses used by a program into the physical addresses in RAM. The program's instructions and data are still physically located on the disk (in `example.exe`), but they are loaded into RAM in chunks (**pages**) as the program runs. The virtual memory map tells the CPU where to find those pages in RAM.

**Why is secure using virtual memory?**

Each process gets its own private, isolated virtual address space. This means one program cannot directly read or write to the memory of another program. If a program has a bug that causes it to write to a bad memory address, it will only corrupt its own virtual space, and the OS will terminate it, preventing it from crashing the entire system or other applications. This is a fundamental security boundary and is why techniques like [[Process Injection]] are necessary. A malicious program must first get a foothold inside the target process's virtual memory space to manipulate it.

#### 🖊️ How Pages Work

The OS uses a **page table** to map virtual pages to physical pages (also called **page frames**).

1. When a program tries to access a virtual address, the CPU and OS work together to look up that address in the page table.
    
2. The page table translates the virtual page number to the corresponding physical page frame number.
    
3. The CPU then uses this physical address to access the data in the RAM.


#### 📔 Memory Allocation APIs: used to allocate virtual memory for a process

A program cannot directly allocate virtual memory itself. A program can, however, **request** that the OS allocate memory for it. It does this by calling specific functions provided by the OS, known as **memory allocation APIs**.

Windows provides multiple APIs that can be used to allocate and free virtual memory for a process, of which there are three main families:

- #### Virtual APIs
    
    These are the lower-level APIs that are used for general (de)allocations and include functions such as [VirtualAlloc](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc), [VirtualFree](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualfree), and [VirtualProtect](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect).  Even though these accept a 'size' argument, the value is always rounded up to the nearest complete page.
    
- #### Heap APIs
    
    These APIs include [HeapAlloc](https://learn.microsoft.com/en-us/windows/win32/api/heapapi/nf-heapapi-heapalloc), [HeapReAlloc](https://learn.microsoft.com/en-us/windows/win32/api/heapapi/nf-heapapi-heaprealloc), and [HeapFree](https://learn.microsoft.com/en-us/windows/win32/api/heapapi/nf-heapapi-heapfree), and are used to manage memory allocations that are smaller than a page.  The heap manager can be thought of an abstraction over the aforementioned virtual APIs.  It allocates pages of memory but optimises its usage by managing smaller allocations within those pages.
    
- #### Memory-mapping APIs
    
    These APIs are designed to map files into memory from disk, and even share those mappings across processes.  These include [CreateFileMappingA](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createfilemappinga), [OpenFileMappingA](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-openfilemappinga), and [MapViewOfFile](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-mapviewoffile).


Knowing how a non-malicious process works, now let's check how a malicious one does:
[[Executing a Beacon vs Process Injection]]


### Properties
---
📆 created   {{16-08-2025}} 15:47
🏷️ tags: #redteam #crto 

---

