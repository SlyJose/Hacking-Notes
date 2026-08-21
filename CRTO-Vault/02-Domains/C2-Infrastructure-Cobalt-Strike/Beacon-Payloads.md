---
domain: c2-infrastructure-cobalt-strike
tags: [beacon, payload, staged, stageless, loader, shellcode, pinvoke]
tools: [Cobalt-Strike]
opsec: high
exam-relevance: core
---

# Beacon Payloads

## Loader Types

### Stomped Loader (pre-CS 4.11)
Based on Stephen Fewer's reflective DLL loading. The DLL exports a `ReflectiveLoader` function that walks its own image, maps a copy into memory, and calls its entry point. A shellcode stub is written over the PE's DOS header to jump to `ReflectiveLoader`.
![[Beacon Loading Process.png]]
### Prepended Loader (CS 4.11+)
Based on DoublePulsar (NSA/Shadow Brokers). The loader is **prepended** to the front of the PE rather than embedded inside it.

![[Prepend loader.png]]

Advantages over stomped:
- Does not overwrite DOS headers
- Can reflectively load any PE (no required exports)
- Supports encoding/encryption/compression of the PE itself
- More obfuscation options

---

## Staged vs Stageless

| | Staged | Stageless |
|--|--------|-----------|
| **Size** | ~890 bytes (stager only) | ~307 KB (full Beacon) |
| **Delivery** | Stager fetches full payload over HTTP at runtime | Full Beacon embedded in payload |
| **Memory** | Stager uses RWX (every byte saved matters) | RW first → flipped to RX before execution |
| **Payload security** | No team server validation — susceptible to hijacking | Public key embedded; session key encrypted in metadata |
| **Flexibility** | Cannot set guardrails or advanced options | Full configuration options |

**OPSEC preference: stageless.** RWX allocation is an EDR signal; stageless payloads use RW→RX. Stagers trade stealth for size.

---

## Payload Types (Payloads menu)

| Type | Format | Notes |
|------|--------|-------|
| HTML Application | `.hta` (HTML + VBScript) | Phishing payload; always x86 |
| MS Office Macro | VBA macro | Paste into Office doc; always x86 |
| Stager Generator | Source (C, Python, etc.) | Byte array for custom shellcode runners |
| Stageless Generator | Source | Same as above with full configuration options |
| Windows Stager/Stageless | Pre-built `.exe`, `.dll`, Service `.exe` | Can be code-signed |
| Generate All Payloads | All stageless variants | Every listener × x86/x64 |

---

## Stageless Generator Options

**Exit Function:**

| Option | API called | When to use |
|--------|-----------|-------------|
| `xprocess` | `ExitProcess` | Beacon is running in a process you spawned |
| `xthread` | `ExitThread` | Beacon is injected into an existing process |

**System Call:**

| Option | Behaviour |
|--------|-----------|
| `Direct` | Calls the `Nt*` kernel function directly |
| `Indirect` | Jumps to the appropriate instruction inside the `Nt*` function |

Both bypass user-mode API hooks placed by EDR on the standard Win32 (`Kernel32`) wrappers.

---

## Related Notes

- [[Cobalt-Strike-Overview]]
- [[Beacon-Listeners]]
- [[PInvoke-CSharp]]
- [[Process-Injection-Overview]]
- [[02-Domains/Defense-Evasion-OPSEC/MOC]]
