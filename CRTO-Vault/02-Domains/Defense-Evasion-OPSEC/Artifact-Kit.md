---
domain: defense-evasion-opsec
tags: [artifact-kit, av-evasion, cobalt-strike, ThreatCheck, ghidra, signatures]
tools: [Cobalt-Strike, ThreatCheck, Ghidra]
opsec: high
exam-relevance: core
---

# Artifact Kit

## What It Is

The Artifact Kit provides the **source code** for Cobalt Strike's stage0 payload templates — the shellcode injectors that get patched with Beacon shellcode to produce `.exe`, `.svc.exe`, and `.dll` payloads. Because these templates are static and shared across CS deployments, they are a primary AV signature target.

## Full Workflow

> 1. **Build** — `build.sh` compiles templates containing **no shellcode** (empty injectors)
> 2. **Test** — even without shellcode, AV can flag the template code itself (static signatures in the injector logic)
> 3. **Clean** — use ThreatCheck to find and modify signatured code until templates pass
> 4. **Load** — load `artifact.cna` via Script Manager; CS now uses your modified templates when generating payloads
> 5. **Generate** — new payloads produced from this point use the cleaned templates with shellcode patched in

## Artifact Naming Convention

| Suffix | Meaning |
|--------|---------|
| `32` / `64` | 32-bit / 64-bit |
| `big` | Stageless (no suffix = staged) |
| `svc` | Windows Service binary |

Example: `artifact64big.exe` = 64-bit stageless executable.

## Kit Structure

![[Artifact Kit Structure.png]]

| Location | Contents |
|----------|---------|
| `src-main/main.c`, `svcmain.c`, `dllmain.c` | Entry points for .exe, .svc.exe, .dll |
| `src-common/bypass-*.c` | Anti-sandbox bypass templates — pick one per build |
| `src-common/patch.c` | Core shellcode injection logic |
| `src-common/injector.c`, `start_thread.c` | Helper functions |
| `README.md` | Explains each bypass technique |

## Build Script

```bash
./build.sh <techniques> <allocator> <stage_size> <rdll_size> <resource_file> <stack_spoof> <syscalls> <output_dir>
```

| Parameter | Options / Notes |
|-----------|----------------|
| `techniques` | Space-separated list of bypass templates (e.g. `mailslot`) |
| `allocator` | `HeapAlloc`, `VirtualAlloc`, `MapViewOfFile` |
| `stage_size` | Reserved space for Beacon shellcode — run `./build.sh` (no args) to get recommended minimums |
| `rdll_size` | Set to `0` unless using a custom loader |
| `resource_file` | `true` to modify binary metadata (CompanyName, FileDescription, etc.) via `src-main/resource.rc` |
| `stack_spoof` | `true` to hide shellcode executing from an unbacked memory region |
| `syscalls` | `none`, `embedded`, `indirect`, `indirect_randomized` |

### Example Build

```bash
./build.sh mailslot VirtualAlloc 344564 0 false false none /mnt/c/Tools/cobaltstrike/custom-artifacts
```

Output: all artifact variants + `artifact.cna` (Aggressor script to load into CS).

> Always run `./build.sh` with no arguments first to confirm the minimum recommended stage size for the current CS version.

---

## Testing: ThreatCheck

**Do not upload to VirusTotal** — it shares artifacts with AV vendors. Use ThreatCheck locally instead. It splits a binary into chunks and scans each with Windows Defender (cloud submission disabled) to find the smallest signatured region.

```powershell
ThreatCheck.exe -f .\artifact64big.exe
# [+] No threat found!        ← clean
# [!] Identified end of bad bytes at offset 0x9CE   ← signatured
```

---

## Identifying Signatured Code: Ghidra

![[Ghidra Analysis.png]]

1. Import the artifact into Ghidra and run default analysis
2. **Navigation > Go To** → enter `file(0x9CE)` (use the offset from ThreatCheck)
3. Identify the function at that address
4. Cross-reference the hex bytes from ThreatCheck output to pinpoint the exact lines
5. Trace back to the corresponding source file in the kit

---

## Bypassing the Signature

**Strategy:** rewrite the signatured code so it compiles to different machine code but performs the same logic. Renaming variables is not enough — variable names don't survive compilation.

### Example: for loop → backwards while loop

```c
/* Original — signatured */
for (int x = 0; x < length; x++) {
    *((char *)ptr + x) = *((char *)buffer + x) ^ key[x % 8];
}

/* Replacement — produces different assembly */
int x = length;
while (x--) {
    *((char *)ptr + x) = *((char *)buffer + x) ^ key[x % 8];
}
```

Comment out the old code rather than deleting it — makes rollback easy if the new version breaks something.

Rebuild, retest with ThreatCheck. Repeat for any remaining detections.

---

## Loading into Cobalt Strike

**CS client → Cobalt Strike > Script Manager → Load → select `artifact.cna`**

CS will now use the custom templates when generating payloads. Test on your own attacking machine before deploying on a target.

> This methodology applies to all artifact types (.exe, .dll, .svc.exe) — follow the same ThreatCheck → Ghidra → modify → rebuild loop for each.

## Related Notes

- [[Defense-Evasion-Overview]]
- [[Resource-Kit]]
- [[Malleable-C2]]
- [[02-Domains/C2-Infrastructure-Cobalt-Strike/Beacon-Payloads]]
- [[02-Domains/Malware-Essentials/PE-File-Structure]]
