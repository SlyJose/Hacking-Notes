---
domain: c2-infrastructure-cobalt-strike
tags: [listener, beacon, http, dns, smb, tcp, egress, p2p, malleable-c2]
tools: [Cobalt-Strike]
opsec: medium
exam-relevance: core
---

# Beacon Listeners

A listener defines the protocol and parameters by which a Beacon communicates back to the team server. Must be configured before generating any payloads.

## Listener Types

| Type | Protocols | Communicates with team server? |
|------|-----------|-------------------------------|
| **Egress** | HTTP, HTTPS, DNS | Directly (crosses network boundary) |
| **Peer-to-Peer (P2P)** | SMB, TCP | No — routes through another Beacon |

P2P Beacons chain together but must ultimately link to an egress Beacon to reach the team server.

---

## HTTP Listener

Beacon communicates via HTTP GET (fetch tasks) / POST (send results). Team server runs a built-in web server.

### Key Configuration

**HTTP hosts** — where Beacon sends requests (IP or domain). Can point directly to team server or to a **redirector** (intermediary that proxies traffic). Common redirector software: `iptables`, `socat`, Apache, NGINX.

**Host rotation strategy** (when multiple hosts configured):

| Strategy | Behaviour |
|----------|-----------|
| `round-robin` | Cycles through list top-to-bottom, one request per host |
| `random` | Randomly selects a host per request |
| `failover-x` / `failover-m/h/d` | Stays on one host until x consecutive failures or failures over given time period |
| `rotate-m/h/d` | Uses each host for the given time period before moving on |

**Max retry strategy** (when all hosts are lost):

| Strategy | Behaviour |
|----------|-----------|
| `none` | Beacon runs indefinitely — must be terminated externally |
| `exit-[max]-[increase]-[duration]` | Increases sleep after `increase` failures, exits after `max` failures. Example: `exit-50-25-1h` = sleep jumps to 1h after 25 failures, exits after 50. |

**HTTP port (C2)** — port Beacon connects to (typically 80/443).  
**HTTP port (bind)** — port the team server binds to. Set differently from C2 port for **port bending** (redirector listens on 80, forwards to non-standard port — useful for running multiple listeners on one team server).

**Guardrails** — stageless payloads only run if criteria match: IP, username, hostname, domain. Prevents payloads running outside the target environment (sandboxes, forwarded phishing emails).

**HTTP host (stager)** — single host used by stager payloads only. Stagers can only use one host.

**Profile** — select Malleable C2 traffic variant.

---

## DNS Listener

Beacon communicates via DNS A, AAAA, or TXT record lookups. Team server runs a built-in DNS server.

**Requirement:** team server must be authoritative for the configured subdomain(s) — set NS records at the registrar.

**Ghost sessions:** DNS Beacons do not send full metadata on initial check-in (bandwidth constraint). They appear as empty rows with a black icon. Issue `checkin` to force metadata transmission.

**DNS resolver** — defaults to the system's configured resolver; can be overridden with a custom IP.

---

## SMB Listener

P2P listener — team server is **not** bound to any port. Serves as a payload generation template only.

Beacon creates a **named pipe** using the configured pipe name. Another Beacon connects to that pipe and relays traffic to the team server over port 445.

Default pipe name: `msagent_##` (random hex). Can be customised — must not clash with existing pipes on the host. Multiple SMB listeners with different pipe names are supported.

---

## TCP Listener

P2P listener — team server is **not** bound to any port.

Beacon binds and listens on the configured C2 port. Another Beacon connects to relay traffic.

| Bind address | Use case |
|--------------|----------|
| `0.0.0.0` | Accepts external connections → lateral movement |
| `127.0.0.1` | Localhost only → privilege escalation within the same host |

Multiple TCP listeners with different ports and bind configs are supported.

---

## Related Notes

- [[Cobalt-Strike-Overview]]
- [[Beacon-Payloads]]
- [[02-Domains/Defense-Evasion-OPSEC/MOC]]
