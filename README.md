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

Simulated a brute-force RDP attack using Hydra with a 262-password wordlist 
against a Windows victim VM. No valid password was found, but the attack ran 
completely unopposed — no account lockout, no rate limiting, no automatic 
defense. Detected and confirmed via Splunk log analysis (Event ID 4625), 
tracing all failed attempts to the attacker's source IP and target account.

## Screenshots

<img width="577" height="709" alt="Hydra brute-force attack execution against RDP" src="https://github.com/user-attachments/assets/c5b1bb85-9687-411e-9061-417e98c7b422" />



*262 password attempts executed against the Windows victim VM via RDP*

<img width="721" height="569" alt="Splunk event log showing failed logon from attack source" src="https://github.com/user-attachments/assets/dff633c9-1746-4a7b-b1cc-0dc63d6e7f98" />
<img width="723" height="570" alt="Failure details confirming source IP and NTLM authentication" src="https://github.com/user-attachments/assets/0ac16a9a-55c4-4b7b-8c38-14d76cab9fe7" />




*Splunk identifying the attack via Event ID 4625, sourced from the Kali attacker VM*

## Full Documentation
- 📋 [Full Incident Report](incident-report.md) — detailed timeline, investigation, and recommendations
- ⚙️ [Lab Setup](setup.md) — environment, tools, and configuration

## What I Learned

Built and debugged a fully isolated lab network from scratch, including 
diagnosing multi-layer connectivity issues — static IP resets, firewall scope 
restrictions, and VirtualBox promiscuous mode settings — that had nothing to 
do with the attack itself but everything to do with real infrastructure 
troubleshooting. Also learned that offensive tools like Hydra have real 
limitations against modern protocols; both RDP and initial SMB attempts 
failed due to tooling issues rather than attacker error, which required 
pivoting strategy mid-investigation rather than assuming the target was secure.

## Next Steps

Planning to add a second scenario simulating a different attack vector 
(e.g., malware execution and process-based detection) to demonstrate 
detection beyond authentication logs.
