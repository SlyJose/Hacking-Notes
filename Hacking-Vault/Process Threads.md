
A thread is a type of object that Windows schedules for execution within a process.  It holds the state of the CPU, including its registers; and a call stack, which is the set of CPU instructions to execute.  Every functional program will have at least one thread that is used to execute the program's entry point, however, many applications run multiple threads to allow multiple pieces of work to execute in parallel.

The simplest thread creation functions are [CreateThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread), which creates a new thread in the calling process; and [CreateRemoteThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread), which can create a new thread in another process.  The most important argument is the function pointer as this acts as the new thread's entry point for execution.  Both of these APIs call into [CreateRemoteThreadEx](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethreadex), which then calls the NtCreateRemoteThreadEx kernel function.

#### 🖊️ A


#### 📔 B


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{16-08-2025}} 15:46
🏷️ tags: #redteam #crto 

---

