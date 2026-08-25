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
