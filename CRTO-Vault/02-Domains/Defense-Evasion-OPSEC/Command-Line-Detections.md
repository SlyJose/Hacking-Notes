---
domain: defense-evasion-opsec
tags: [command-line, detection, CreateProcess, kernel-callbacks, OPSEC, bypass, pth, uac]
tools: [Cobalt-Strike, Mimikatz]
opsec: high
exam-relevance: core
---

# Command-Line Detections

Many Beacon commands that spawn new processes go through the `CreateProcessA` API. Windows Defender hooks into kernel callbacks to inspect those calls — and blocks them by returning `STATUS_ACCESS_DENIED` if the command line matches a known-bad pattern.

---

## How `CreateProcessA` Works

```c
BOOL CreateProcessA(
  [in, optional]      LPCSTR                lpApplicationName,
  [in, out, optional] LPSTR                 lpCommandLine,
  ...
  [in]                LPSTARTUPINFOA        lpStartupInfo,
  [out]               LPPROCESS_INFORMATION lpProcessInformation
);
```

Two string parameters matter for detection:
- **`lpApplicationName`** — explicit path to the executable (can be NULL)
- **`lpCommandLine`** — the full command line string; when `lpApplicationName` is NULL, the OS parses the binary name from here

---

## How Beacon Commands Populate These Parameters

| Command | `lpApplicationName` | `lpCommandLine` |
|---------|-------------------|----------------|
| `shell` | NULL | `C:\Windows\System32\cmd.exe /C <your command>` |
| `run` | NULL | `<your command>` |
| `powershell` | NULL | `powershell -nop -exec bypass -EncodedCommand <b64>` |
| fork & run (spawn) | NULL | `<current spawnto>` |

> The `powershell` command line can be modified via the `POWERSHELL_COMMAND` Aggressor hook if you need to change the default flags or encoding.

---

## Detection Examples

### `pth` (Pass-the-Hash)

`pth` is a wrapper around Mimikatz's `sekurlsa::pth`. Internally, Mimikatz uses `CreateProcessWithLogonW` — a named pipe impersonation technique — to start `cmd.exe` with the stolen token.

```beacon
beacon> pth CONTOSO\rsteel FC525C9683E8FE067095BA2DDC971889
[*] Tasked beacon to run mimikatz's sekurlsa::pth /user:"rsteel" /domain:"CONTOSO" /ntlm:FC525C9683E8FE067095BA2DDC971889 /run:"%COMSPEC% /c echo 3653adf6614 > \\.\pipe\c0b81c" command
[+] [job 0] received output:
user    : rsteel
domain  : CONTOSO
program : C:\Windows\system32\cmd.exe /c echo 3653adf6614 > \\.\pipe\c0b81c
impers. : no
NTLM    : fc525c9683e8fe067095ba2ddc971889
ERROR kuhl_m_sekurlsa_pth ; CreateProcessWithLogonW (0x00000005)

beacon> windows_error_code 5
ERROR_ACCESS_DENIED
```

The output tells you exactly what Mimikatz tried to spawn and which API was blocked. The command line `cmd.exe /c echo ... > \\.\pipe\...` is a known Mimikatz indicator — Defender's callback sees it and returns `STATUS_ACCESS_DENIED`.

---

### `uac-schtasks` (UAC Bypass via Elevate Kit)

`uac-schtasks` is a UAC bypass from Cobalt Strike's Elevate Kit that escalates a medium-integrity session to high-integrity. The kit imports `Invoke-EnvBypass.ps1`, which internally calls `Start-Process` to run `schtasks.exe` with the argument `/Run /TN \Microsoft\Windows\DiskCleanup\SilentCleanup /I`.

```beacon
beacon> elevate uac-schtasks tcp-local
[*] Tasked Beacon to run windows/beacon_bind_tcp (127.0.0.1:1337) in a high integrity context
[+] [job 0] received output:
ERROR: Start-Process : This command cannot be run due to the error: Access is denied.
ERROR: At line:77 char:21
ERROR: + ...  $Process = Start-Process -FilePath $schtasksPath -ArgumentList '/Run ...
```

`schtasks.exe` combined with `SilentCleanup` on the command line is a well-known UAC bypass indicator — the driver blocks it. Reading the kit's Aggressor script (`beacon_exploit_register` → `schtasks_exploit`) reveals the full execution chain when diagnosing why a bypass failed.

---

## Why the Error is "Access Denied" (Not Something More Specific)

Windows Defender registers a kernel callback via `PsSetCreateProcessNotifyRoutineEx`. When a process is about to be created, the callback receives a `PPS_CREATE_NOTIFY_INFO` structure:

| Member | Contents |
|--------|---------|
| `ImageFileName` | Path to the executable being started |
| `CommandLine` | Full command line string |
| `CreationStatus` | NTSTATUS the driver can set to block the call |

If the driver dislikes the `CommandLine` value, it sets `CreationStatus = STATUS_ACCESS_DENIED` — which surfaces to the caller as `ERROR_ACCESS_DENIED` (0x5). There's no separate "blocked by AV" error code; it looks identical to a permission error.

---

## Bypasses

You **cannot disable** `PsSetCreateProcessNotifyRoutineEx` callbacks from user space. Removing them requires kernel-level code execution (kernel exploit or vulnerable driver), which is out of scope for CRTO.

The viable approach is **procedure substitution** — find a different method that achieves the same outcome without the flagged command line:

- Instead of `schtasks.exe /Run /TN SilentCleanup`, find a **COM object or API** that triggers the scheduled task directly (no `schtasks.exe` on the command line at all).
- Instead of Mimikatz `pth`, look for alternative pass-the-hash implementations that don't use `CreateProcessWithLogonW` in the same pattern.

> Ask yourself: does the goal require this specific binary and these specific arguments, or is that just the most obvious path? If it's the latter, there's usually a quieter one.

---

## Related Notes

- [[Beacon-Command-Behavior]] — full breakdown of command classes and which ones spawn processes
- [[Blending-Post-Ex]] — using `ppid` and `spawnto` to blend process spawning
- [[AMSI-Bypass]] — analogous kernel-level detection for script content
- [[Defense-Evasion-Overview]]
