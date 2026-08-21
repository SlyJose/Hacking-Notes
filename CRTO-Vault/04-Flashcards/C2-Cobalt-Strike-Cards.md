---
tags: [flashcards, c2-infrastructure-cobalt-strike]
---

# C2 Infrastructure & Cobalt Strike Flashcards

## Overview & Architecture

What are the three components of Cobalt Strike?::Beacon (agent/implant), Team Server (control + logging), Client (operator interface connecting to team servers).

What is the distributed operations design pattern in Cobalt Strike?::One team server per engagement phase (initial access, post-exploitation, persistence) — if one channel is burned, others survive.

## Listeners

What is the difference between an Egress and a P2P listener?::Egress (HTTP/HTTPS/DNS) communicates directly to the team server. P2P (SMB/TCP) routes through another Beacon.

What is a redirector in the context of Cobalt Strike?::An intermediary host between Beacon and the team server that proxies C2 traffic. Common software: iptables, socat, Apache, NGINX.

What does the `exit-50-25-1h` max retry strategy mean?::After 25 consecutive failures, increase sleep to 1h. After 50 consecutive failures, Beacon exits.

What is port bending and why use it?::Setting HTTP port (C2) and HTTP port (bind) to different values. Lets a redirector listen on port 80 while the team server binds to a non-standard port — enables multiple listeners on one server.

What do guardrails do?::Prevent stageless payloads from running unless the host matches configured criteria (IP, username, hostname, domain) — stops payloads executing in sandboxes or forwarded phishing chains.

Why do DNS Beacons initially appear as ghost sessions?::DNS has limited bandwidth — Beacon doesn't transmit its metadata on first check-in. Use `checkin` to force metadata send.

What does an SMB Beacon create when executed?::A named Windows pipe (default: `msagent_##`). Another Beacon must connect to this pipe to relay traffic over port 445.

When would you bind a TCP listener to 127.0.0.1 vs 0.0.0.0?::127.0.0.1 (localhost only) for privilege escalation within the same host. 0.0.0.0 for lateral movement (accepts external connections).

## Payloads

What is the key difference between the stomped and prepended loaders?::Stomped writes a shellcode stub over the DOS header and requires `ReflectiveLoader` export. Prepended attaches the loader before the PE — no DOS header modification, no required exports, supports PE obfuscation.

What are the OPSEC differences between staged and stageless payloads?::Stagers use RWX memory (EDR signal). Stageless uses RW → RX flip before execution. Stageless also embeds the team server's public key for mutual authentication.

Approximately how large is a Beacon stager vs a full stageless Beacon?::Stager: ~890 bytes. Stageless: ~307 KB (~345× larger).

Why are stagers susceptible to hijacking?::They have no mechanism to validate the team server — any host listening on the expected IP/port can serve a malicious stage.

What is the difference between `xprocess` and `xthread` exit functions?::`xprocess` calls `ExitProcess` — terminates the entire host process. `xthread` calls `ExitThread` — only kills the Beacon thread. Use `xthread` when injected into an existing process.

What is the difference between Direct and Indirect system calls in Cobalt Strike?::Direct calls the `Nt*` kernel function directly. Indirect jumps to the appropriate instruction inside the `Nt*` function. Both bypass user-mode EDR hooks on Win32 wrappers.

## Interacting with Beacon

What does a red monitor icon with an asterisk on the username mean?::Beacon is running with high integrity (local admin or SYSTEM privileges).

What does the `checkin` command do for a DNS Beacon?::Forces the Beacon to send its metadata to the team server, populating the ghost session row.

In the session graph view, what do solid yellow lines represent?::SMB P2P connections between Beacons.

In the session graph view, what do dashed green lines represent?::HTTP/S egress connections to the team server.
