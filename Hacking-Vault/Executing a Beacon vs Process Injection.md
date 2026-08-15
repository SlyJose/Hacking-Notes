
#### Executing a beacon

When you run `beacon.exe`, you are executing the Cobalt Strike beacon shellcode. The shellcode is executed in the **current process**, not in a separate, remote one.

Here is a step-by-step breakdown of how this happens:

##### 1. Initial Execution

When you double-click `beacon.exe`, the operating system's loader starts a new process. This process has its own virtual memory space, stack, heap, and so on. The `.text` section of the `beacon.exe` PE file, which contains the program's code, is loaded into this new process's virtual memory and mapped to physical memory (RAM).

##### 2. The Loader

The `beacon.exe` file is essentially a **loader stub**. Its primary job is to create a suitable environment and then execute the embedded beacon shellcode. This loader is usually very small and simple. It often contains a decryption routine for the shellcode, which is often encrypted to evade antivirus signatures.

##### 3. Shellcode Execution

The loader allocates a new region of memory within its **own virtual memory space**. It then decrypts and copies the actual beacon shellcode into this new, allocated memory region. The loader then transfers execution to the start of the shellcode. This can be done with a simple call or jump instruction.

##### 4. Beacon Operations

Once the shellcode gains control, it performs its malicious functions:

- It resolves the necessary API functions (e.g., `LoadLibraryA`, `InternetConnectA`, `CreateProcessA`) by walking the in-memory data structures, a technique often called **manual PE parsing** or **API hashing**.
    
- It then establishes a connection back to the Command and Control (C2) server.
    
- It enters a loop to check for tasks and executes commands received from the C2 server.
    

#### Process Injection

The goal is to get your malicious code to run in a different process, often a trusted one like `explorer.exe` or `svchost.exe`, to evade detection and inherit its privileges.

Let's remember ([[Process Memory]]) 

- **Virtual Memory Page** is an entry in a **map**. It tells the operating system: "Hey, at this logical address, I've got something important."
    
- **Physical Memory** (RAM) is the **actual storage unit**. It's the physical warehouse where your shellcode gets put.
    
When we want to inject another process, we call `VirtualAllocEx`. When you use `VirtualAllocEx`, you are **creating a new entry in the map** and telling the OS to **prepare a space** for your shellcode. The OS reserves a virtual memory page and flags it, but the shellcode isn't there yet.

Then we call `WriteProcessMemory`.

When you use `WriteProcessMemory`, you are **filling that space** with the shellcode. The OS sees that you are writing to a virtual address, looks at the map, finds the corresponding physical memory location (a page frame in RAM), and writes your shellcode to that spot.

So, you're not putting the shellcode "in" the virtual memory page itself. You're putting it in the physical memory location **that the virtual memory page points to**.

`VirtualAllocEx` followed by `WriteProcessMemory` and `CreateRemoteThread` is specifically designed for this. The `Ex` in `VirtualAllocEx` stands for "extended" and allows you to specify a handle to a different process, enabling you to allocate memory and write to it remotely.


#### ⚠ Opsec




### Properties
---
📆 created   {{16-08-2025}} 16:51
🏷️ tags: #redteam #crto 

---

