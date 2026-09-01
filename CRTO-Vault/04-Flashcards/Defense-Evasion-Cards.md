---
tags: [flashcards, defense-evasion-opsec]
---

# Defense Evasion & OPSEC Flashcards

## Overview

What are the two core AV detection strategies?::1. Signature-based (targets static portions of payload at load/execution time). 2. Behavioural heuristics (monitors post-exploitation actions like process spawning, injection, shell commands).

## Artifact Kit

What does the Artifact Kit provide?::Source code for Cobalt Strike's stage0 payload templates (.exe, .svc.exe, .dll) — shellcode injectors patched with Beacon shellcode.

What does `artifact64big.exe` refer to?::64-bit stageless executable artifact. `big` = stageless, `64` = 64-bit.

What file contains the core shellcode injection logic in the Artifact Kit?::`src-common/patch.c`

What is the purpose of `bypass-*.c` files in src-common?::Anti-sandbox bypass templates — select one per build to include in the artifacts.

What does the `resource_file` build parameter do?::When `true`, embeds custom metadata (CompanyName, FileDescription, etc.) into the binary via `resource.rc`.

Why should you run `./build.sh` with no arguments before building?::To get the minimum recommended stage size for the current Cobalt Strike version.

Why should you NOT upload test artifacts to VirusTotal?::It shares samples with AV vendors, burning your custom artifacts.

What tool is used instead of VirusTotal to find signatured bytes locally?::ThreatCheck — splits the binary into chunks and scans each with Windows Defender (cloud submission disabled).

How do you jump to a ThreatCheck-reported offset in Ghidra?::Navigation > Go To → enter `file(0xOFFSET)` (e.g. `file(0x9CE)`).

What is the Artifact Kit bypass strategy for signatured code?::Rewrite the code so it compiles to different machine code while keeping the same functionality. Variable renaming alone is not enough — names don't survive compilation.

How do you load custom artifact templates into Cobalt Strike?::Cobalt Strike > Script Manager > Load → select the generated `artifact.cna` file.

---
## AMSI & Resource Kit

What is AMSI and who decides if content is malicious?::AMSI (Antimalware Scan Interface) is a vendor-agnostic Windows bridge that submits content to the installed AV product. The AV engine decides — not AMSI itself.

Name four native Windows components that are AMSI-aware.::PowerShell, Windows Script Host (wscript/cscript), JavaScript/VBScript, Office VBA macros.

What is the preferred AMSI bypass approach over memory patching?::Identify which portion of the script triggers detection via ThreatCheck (`-e AMSI -t Script`), then modify that code to break the signature while keeping the same functionality.

What ThreatCheck command scans a PS1 file against AMSI?::`ThreatCheck.exe -f .\template.x64.ps1 -e AMSI -t Script`

What is the Resource Kit equivalent to the Artifact Kit?::The Resource Kit provides source templates for script-based payloads (.ps1, .vbs, .hta) — same concept as the Artifact Kit but for scripts instead of compiled binaries.

What is the Resource Kit build command?::`./build.sh <output_dir>` — no other required arguments.

What template does `jump winrm64` use when generating its payload?::`template.x64.ps1`

Why can ThreatCheck not fully test compress.ps1?::AMSI evaluates PowerShell in layers — scanning output at each evaluation step (e.g. after base64 decode). ThreatCheck only scans the file statically and cannot replicate layered evaluation.

What tool is used to obfuscate compress.ps1 when ThreatCheck can't reproduce the detection?::Invoke-Obfuscation (`Invoke-Obfuscation> SET SCRIPTBLOCK '...'`, then apply token obfuscation).

What must NOT be changed in compress.ps1 when obfuscating?::The `%%DATA%%` placeholder — Cobalt Strike uses this to patch shellcode into the correct location in the script.

---
## Beacon Memory

Why is RWX memory a detection signal?::Most native processes don't use PAGE_EXECUTE_READWRITE. Its presence is anomalous and flagged by memory scanners.

What does `stage.userwx false` cause the prepended loader to do differently?::Allocates RW memory, copies Beacon in, then sets per-section permissions: .text→RX, .rdata→R, .data→RW. Avoids RWX entirely.

What does `stage.copy_pe_header false` do?::Prevents the loader from copying Beacon's DOS/NT headers into memory — they're only needed during loading, not at runtime.

Why does Beacon's memory region look suspicious without module stomping?::Legitimate RX memory is backed by a module on disk. Beacon loaded from memory has no disk-backed module — a visible anomaly in tools like System Informer.

What does `stage.module_x64 "Hydrogen.dll"` do?::Tells the loader to map Hydrogen.dll from disk into memory, then overwrite it with Beacon — making Beacon appear backed by a legitimate module.

What size constraint applies when choosing a module for module stomping?::The module must be at least as large as Beacon (recommended ≥512KB). Pick from System32.

How do you export the raw Beacon DLL for string analysis?::Load a `beacon.cna` using the `BEACON_RDLL_GENERATE` hook to write `$2` (the raw DLL) to disk, then trigger it with `artifact_payload()`.

What command dumps strings from the raw Beacon DLL?::`strings -d beacon_raw.x64.dll -n 6`

What are the two constraints on `strrep` replacements in Malleable C2?::Replacement must be ≤ length of original. Format specifiers must remain valid (%s, %d, etc.).

Which strings must NEVER be replaced with strrep and why?::Internal webserver strings like `HTTP/1.1 200 OK` — Beacon uses these for powershell-import, powerpick, and psinject. Replacing them breaks HTTP compliance and those workflows entirely.

---
## Beacon Command Behavior

What determines the OPSEC cost of a Beacon command?::Its command class — each class triggers different underlying actions (API calls, shellcode injection, new threads, named pipes, new processes).

Which Beacon command class has the lowest OPSEC burden?::House-keeping and API-only commands — they execute within Beacon using native APIs with no new threads, shellcode, or processes.

What are BOFs and how do they execute?::Beacon Object Files — compiled C programs sent down with tasking data and executed within Beacon's main thread. No new thread is created; memory is cleared after execution.

What three things must every fork & run command do?::Inject shellcode into a process, create a new thread, and open an SMB named pipe. The spawn variant also starts a new sacrificial process.

What is the difference between fork & run "explicit" and "spawn"?::Explicit injects into an already-running process. Spawn starts a new sacrificial process first, then injects.

Which commands are spawn-only vs explicit-only?::Spawn-only: `execute-assembly`, `powerpick`. Explicit-only: `psinject`. Both: `mimikatz`, `keylogger`, `portscan`, `printscreen`, `desktop`.

What does address spoofing in the `execute{}` block do?::Creates the thread pointing at a legitimate function address (e.g. `ntdll.dll!RtlUserThreadStart+0x2c`), then redirects it to shellcode — avoids memory scan triggers on shellcode start addresses.

What does `set amsi_disable "true"` in the post-ex block do?::Memory-patches AMSI inside processes used by `powerpick`, `execute-assembly`, and `psinject`.

What is the difference between `strrep` and `strrepex` in the post-ex transform block?::`strrep` replaces a string across ALL post-ex DLLs. `strrepex` targets a specific named post-ex DLL only (e.g. "PowerPick", "ExecuteAssembly").

Why can't `post-ex.spawnto` override the spawnto for service creation commands?::Service payloads run in a SYSTEM context where environment variables like `%windir%` are invalid. Use `ak-settings` from the Artifact Kit to set an explicit path instead.

What does the `shell` command route through vs `powershell`?::`shell` routes through `cmd.exe`; `powershell` routes through `powershell.exe`.
