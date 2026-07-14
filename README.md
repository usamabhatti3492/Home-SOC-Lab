# Home SOC Lab — Detecting a Brute-Force RDP Attack

A small home lab simulating a real security incident end-to-end: attack, detection, investigation, and reporting — mirroring the workflow of a SOC analyst.

## Tools Used
Splunk Free · VirtualBox · Kali Linux · Windows 10 · Hydra

## What This Project Demonstrates
- Setting up an isolated attacker/victim lab environment
- Ingesting and querying logs in a SIEM
- Investigating a security event from raw logs to root cause
- Writing an incident report a real SOC team would recognize

## Key Finding
[2-3 sentences — the short version of what you found. Example: "Simulated a brute-force RDP attack from an external-facing Kali VM. Identified 40+ failed login attempts within 90 seconds via Splunk log analysis, isolated the source IP, and documented the incident with a recommended mitigation (account lockout policy)."]

## Screenshots
![Splunk search results](screenshots/splunk-search.png)
*One-line caption explaining what this shows*

![Attack detection](screenshots/detection.png)
*One-line caption*

## Full Documentation
- 📋 [Full Incident Report](incident-report.md) — detailed timeline, investigation, and recommendations
- ⚙️ [Lab Setup](setup.md) — environment, tools, and configuration

## What I Learned
[2-3 honest sentences about the actual skill gained]

## Next Steps
[Optional — what you're building next, e.g., "Adding a second scenario simulating malware execution."]
