---
domain: c2-infrastructure-cobalt-strike
tags: [beacon, interaction, session, graph-view, commands]
tools: [Cobalt-Strike]
opsec: n/a
exam-relevance: core
---

# Interacting with Beacon

## Session Table — Key Columns

| Column | Description |
|--------|-------------|
| `external` | External IP resolved by the CS web server |
| `internal` | Internal IP of the target host |
| `listener` | Egress listener in use |
| `user` | Username Beacon runs as |
| `computer` | Hostname |
| `process` | Process name Beacon is injected into |
| `pid` | Process ID |
| `arch` | x86 or x64 |
| `last` | Time since last check-in |
| `sleep` | Current sleep interval |

**Integrity indicators:**
- Blue monitor icon = medium integrity (standard user)
- Red monitor icon + `username*` = high integrity (local admin or SYSTEM)

## DNS Beacon Ghost Sessions

DNS Beacons don't send metadata on first check-in (bandwidth constraint). They appear as empty rows with a **black icon**. Run `checkin` to force metadata transmission.

## Interacting

Double-click a session row (or right-click → Interact) to open the Beacon console.

```
beacon> help              # list all commands
beacon> help getuid       # help for a specific command
beacon> getuid            # queue a job
beacon> clear             # clear the job queue
```

Jobs are queued locally and sent in bulk on next check-in. Output is displayed once Beacon returns results.

## Session Graph View

Shows Beacon relationships that the table view obscures. Useful for understanding P2P chains.

**Line styles and colors:**

| Color / Style | Protocol |
|---------------|----------|
| Dashed green | HTTP/S (egress) |
| Dashed yellow | DNS (egress) |
| Solid yellow | SMB (P2P) |
| Solid green | TCP (P2P) |

Firewall icon + dashed line = egress (talking directly to team server). Solid lines = P2P connections.

Right-click the background for layout options. Right-click a Beacon for the interaction menu.

## Related Notes

- [[Cobalt-Strike-Overview]]
- [[Beacon-Listeners]]
- [[Beacon-Payloads]]
