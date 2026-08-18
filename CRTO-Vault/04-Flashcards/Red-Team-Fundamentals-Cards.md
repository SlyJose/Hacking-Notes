---
tags: [flashcards, red-team-fundamentals]
---

# Red Team Fundamentals Flashcards

What is red teaming?::The process of using TTPs to emulate a real-world threat, measuring the effectiveness of people, processes, and technologies defending an environment.

What should a red team operational objective be?::A specific, pre-agreed business goal (e.g. access to a critical system/dataset) — not "get domain admin".

What is a Tactic?::The *why* — overall tactical goal (e.g. Credential Access).

What is a Technique?::The *how* — method used to achieve a tactic (e.g. dump LSASS memory).

What is a Procedure?::The exact step-by-step implementation of a technique (e.g. Mimikatz `sekurlsa::logonpasswords`).

Emulation vs Simulation — what's the key difference?::Emulation mirrors a specific known threat actor (narrow, TI-driven). Simulation uses hypothetical TTPs (broad, exploratory).

Name the 7 phases of the Cyber Kill Chain.::Reconnaissance → Weaponisation → Delivery → Exploitation → Installation → C2 → Actions on Objectives

Name the 8 phases of Mandiant's Targeted Attack Lifecycle.::Initial Recon → Initial Compromise → Establish Foothold → Escalate Privileges → Internal Recon → Move Laterally → Maintain Presence → Complete Mission

What does "primum non nocere" mean for red teamers?::First, do no harm — don't increase client risk (no disabling controls, no real data exfiltration, no unvetted backdoors).

What must always be agreed before a red team engagement begins?::Rules of Engagement (ROE) — authorised targets, restrictions, and objectives.

Why must C2 always be encrypted?::Plaintext traffic is detectable by IDS/IPS, and cleartext credentials are vulnerable to sniffing.
