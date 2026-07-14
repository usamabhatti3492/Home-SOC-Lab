# Home SOC Lab — Detecting a Brute-Force RDP Attack

## Overview
A small home lab built to simulate a real security incident end-to-end: attack, detection, investigation, and reporting — mirroring the workflow of a SOC analyst.

## Tools Used
- VirtualBox (virtualization)
- Kali Linux (attacker machine)
- Windows 10 (victim machine)
- Splunk Free (SIEM / log analysis)
- Hydra (brute-force simulation tool)

## Lab Architecture
[1-2 sentences + a simple diagram or screenshot of your network setup — two VMs on an internal network]

## What I Did
1. Set up an isolated lab network with a Kali attacker VM and a Windows victim VM
2. Configured Splunk to ingest Windows Security Event Logs
3. Simulated a brute-force RDP attack from Kali using Hydra
4. Investigated the resulting logs in Splunk to identify the attack
5. Documented the incident as a formal report (below)

## Detection
[Screenshot of your Splunk search + a short caption explaining what query you ran and what it found]

Example: `index=main EventCode=4625 | stats count by src_ip` — this query surfaces failed login attempts (Event ID 4625) grouped by source IP, which immediately showed [X] failed attempts from [IP] within [timeframe].

## Incident Report

**Summary:** [2 sentences — what happened, in plain language]

**Timeline:**
| Time | Event |
|------|-------|
| 14:02 | First failed login attempt detected |
| 14:03 | 40+ failed attempts from same source IP |
| 14:04 | [Successful login / attack stopped / etc.] |

**Detection Method:** [What log source and query surfaced this]

**Impact:** [What could have happened if this were real / what was at risk]

**Recommendation:** [The control that would prevent or catch this faster next time — e.g., account lockout policy, MFA, rate limiting, alerting rule]

## What I Learned
[2-3 honest sentences — e.g., "I learned how failed authentication events are logged in Windows and how to write basic Splunk search syntax to filter them."]

## Next Steps
[Optional — e.g., "Planning to add a second scenario simulating malware execution and process-based detection."]
