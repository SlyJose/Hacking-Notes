---
domain: defense-evasion-opsec
tags: [ppid, spawnto, post-ex, OPSEC, process-spoofing, fork-and-run]
tools: [Cobalt-Strike]
opsec: high
exam-relevance: core
---

# Blending Post-Ex

The goal is to make post-ex actions look like legitimate activity on the host — both the parent-child process relationships and the sacrificial processes used for fork & run commands must match what the surrounding environment would naturally produce.

> **Key principle:** Your C2 profile `spawnto` is a default, not a guarantee. Every command you run must be evaluated against where Beacon is currently running and what the surrounding process activity looks like.

---

## The Problem: Default Spawning Behavior

Assume Beacon is running inside `msedge.exe` — a reasonable host for an HTTP/S Beacon since Edge makes outbound HTTP requests legitimately.

![[assets/msedge beacon.png]]

Commands like `shell`, `run`, `execute-assembly`, and `powerpick` all spawn new processes. By default those new processes are children of the Beacon's current process — meaning `msedge.exe` spawns `cmd.exe`, `powershell.exe`, etc.

![[assets/shell run powerpick.png]]

Seeing Edge spawn `cmd.exe` or `powershell.exe` is an immediate detection signal for any half-decent EDR or analyst.

---

## Fix 1: PPID Spoofing — `ppid`

`ppid` changes the parent PID that Beacon uses when launching processes via `shell` and `run`. The technique injects into the target parent's process space before spawning — so the new process's PPID appears to be the spoofed parent, not `msedge.exe`.

**Workflow:**
1. Use `ps` or the Process Browser to find a plausible parent PID (e.g. `explorer.exe`).
2. Set it with `ppid`.
3. Run your command — it will appear as a child of the spoofed parent.

```beacon
beacon> ppid 6696
[*] Tasked beacon to spoof 6696 as parent process

beacon> shell timeout 60
[*] Tasked beacon to run: timeout 60
```

![[assets/parent process.png]]

Same technique applies to `run`. Reset to Beacon's own process with `ppid` (no argument):

```beacon
beacon> ppid
[*] Tasked beacon to use itself as parent process
```

> Choose a parent that makes sense for the command. `explorer.exe` is generic and plausible for most user-context activity. `svchost.exe` for system-looking tasks. Match the child to the parent.

---

## Fix 2: Sacrificial Process — `spawnto`

Fork & run commands (`execute-assembly`, `powerpick`, `psinject`, etc.) inject into a **sacrificial process**. `spawnto` controls what that process is.

If Beacon is already running in `msedge.exe`, one option is to spawn another Edge process as the sacrificial process — Edge routinely spawns child Edge processes, so this blends naturally:

```beacon
beacon> ppid
[*] Tasked beacon to use itself as parent process

beacon> spawnto x64 "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --profile-directory=Default
[*] Tasked beacon to spawn x64 features to: "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --profile-directory=Default

beacon> powerpick start-sleep -s 60
```

![[assets/spawnto.png]]

Alternatively, combine both: set `ppid` to `explorer.exe` and `spawnto` to something plausible like `notepad.exe`, `powershell.exe`, or `msiexec.exe` depending on what you're running.

---

## Decision Matrix

| Scenario | Recommendation |
|----------|---------------|
| Beacon in `msedge.exe`, need `shell`/`run` | `ppid` → `explorer.exe` |
| Beacon in `msedge.exe`, need `execute-assembly` | `spawnto` → another `msedge.exe` process, or `ppid explorer` + `spawnto msiexec.exe` |
| Beacon in `msedge.exe`, need `powerpick` | Same as execute-assembly |
| Resetting after targeted op | `ppid` (no arg), `spawnto` back to profile default |

---

## OPSEC Notes

- The `post-ex` block in your Malleable C2 profile sets a **default** `spawnto` — but you can and should override it per-command based on context.
- `ppid` spoofing is detectable if defenders are checking the `PROC_THREAD_ATTRIBUTE_PARENT_PROCESS` attribute — but it still defeats basic parent-child analysis in most SIEMs.
- Running fork & run inside a process that legitimately performs similar operations (e.g. a .NET assembly inside a process that already loads the CLR) is significantly quieter than spawning a new sacrificial process.

---

## Related Notes

- [[Beacon-Command-Behavior]] — command classes and which ones spawn processes
- [[Defense-Evasion-Overview]]
- [[Beacon-Memory]]
- [[02-Domains/C2-Infrastructure-Cobalt-Strike/Interacting-with-Beacon]]
