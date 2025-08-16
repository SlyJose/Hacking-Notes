The DOS stub is only used when the PE file is executed under MS-DOS, which simply prints the message "This program cannot be run in DOS mode".  The modern Windows loader uses the offset in `e_lfanew` to skip over this stub and go directly to the NT headers.

![[DOS Stub.png]]

### Properties
---
📆 created   {{16-08-2025}} 12:35
🏷️ tags: #redteam #crto 

---
