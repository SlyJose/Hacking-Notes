The **Windows API (Application Programming Interface)** is a huge collection of functions provided by Windows for user-mode applications and system services.
    
They’re how a process asks Windows to do something on its behalf—like opening a file, drawing a window, sending network data, or allocating memory.
    
For example:
    
    - `CreateFileW()` → open or create a file
        
    - `VirtualAlloc()` → allocate memory
        
    - `CreateProcessW()` → start a new process
        
    - `WriteFile()` → write data to a file

#### 🚀 - How are APIs called
---

![[Processes Windows APIs.png]]

A process can’t directly manipulate hardware or kernel structures, so it goes through **layers**:

1. **User-mode API call (Win32 API)**
    
    - The application calls a high-level Windows function (like `CreateFileW`).
        
    - This function usually lives in **DLLs** such as `kernel32.dll`, `user32.dll`, or `advapi32.dll`.
        
2. **Forwarding to NTDLL**
    
    - Many Win32 APIs are just wrappers.
        
    - For example, `kernel32.dll!CreateFileW` eventually calls a function in `ntdll.dll` like `NtCreateFile`.
        
    - `ntdll.dll` is the **lowest-level user-mode DLL** that exposes the **Native API**.
        
3. **System Call Transition (User mode → Kernel mode)**
    
    - The Native API function (`NtCreateFile`) executes a **system call instruction** (`syscall` on x64, `int 0x2e` on older systems).
        
    - This traps into the kernel, switching the CPU from **ring 3 (user mode)** to **ring 0 (kernel mode)**.
        
4. **Kernel-mode execution**
    
    - The system call dispatcher in `ntoskrnl.exe` (the Windows kernel) looks up the system call number in a table (`KeServiceDescriptorTable`).
        
    - It routes execution to the correct kernel routine, e.g., `NtCreateFile` in kernel space.
        
    - The kernel then interacts with drivers, the filesystem, or hardware to actually do the work.
        
5. **Return to user mode**
    
    - Once the kernel finishes, it returns the result (like a file handle or error code) back through `ntdll.dll` → `kernel32.dll` → back to your application.


There are multiple APIs that can be used to start a process depending on your requirements.  The simplest is [CreateProcessW](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessw) which creates a process that will have the same access token as the caller; [CreateProcessAsUserW](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessasuserw) can create a process using an alternate access token; and [CreateProcessWithLogonW](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createprocesswithlogonw) can create a process using a user's plaintext credentials.  Ultimately, each API calls into the NtCreateUserProcess kernel function.

---
#### 📦 - B
--- 

#### 🖊️ - C


⚠ Alert 1
⚠ Alert 2
⚠ Alert 3


--- 

 1️⃣ A
 2️⃣ B
 
--- 

❗C


### Properties
---
📆 created   {{16-08-2025}} 15:33
🏷️ tags: #redteam #crto 

---
