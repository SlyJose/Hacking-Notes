---
domain: defense-evasion-opsec
tags: [AMSI, av-evasion, powershell, bypass, memory-patching]
tools: [Cobalt-Strike]
opsec: high
exam-relevance: core
---

# AMSI Bypass

## What is AMSI?

The **Antimalware Scan Interface (AMSI)** is a vendor-agnostic Windows standard that lets applications submit content to an installed AV product for scanning. Any 3rd-party AV can register as an AMSI provider on installation. AMSI is just the bridge — the **AV engine decides** whether content is malicious, not AMSI itself.

![[amsi infrastructure.jpg]]

### Native Components that are AMSI-Aware

- PowerShell (scripts, interactive use, dynamic code evaluation)
- Windows Script Host (`wscript.exe`, `cscript.exe`)
- JavaScript and VBScript
- Office VBA macros
- User Account Control (EXE, COM, MSI, ActiveX elevation)

## Bypass Approaches

### Memory Patching / Hardware Breakpoints
Disrupt the bridge between the application and AV engine by:
- Patching `amsi.dll` in memory to prevent initialisation
- Using hardware breakpoints to intercept and return a fake "no threat" response

**Drawback:** Many of these bypasses are themselves signatured or leave indicators detectable by blue teams.

### Preferred Approach: Modify Signatured Code
Same methodology as the [[Artifact-Kit]]:
1. Identify which portion of the script triggers AV using ThreatCheck (`-e AMSI -t Script`)
2. Modify the offending code to break the signature while preserving functionality
3. Re-scan, repeat until clean

This is more durable than generic bypass tricks and less likely to be caught by AMSI providers themselves.

## Related Notes

- [[Resource-Kit]]
- [[Artifact-Kit]]
- [[Defense-Evasion-Overview]]
