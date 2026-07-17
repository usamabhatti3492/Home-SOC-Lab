# Incident Report: Brute-Force RDP Attack

## Summary
Simulated a brute-force RDP attack against a Windows victim VM using Hydra with 
the fasttrack.txt wordlist (262 password attempts). No valid password was found; 
Splunk log analysis confirmed the failed login attempts and traced them to the 
source IP and timestamp of the attack.

## Timeline
| Time | Event |
|------|-------|
| 15:30:22 | Hydra attack initiated against RDP (port 3389)  |
| 15:30 - 15:32 | 262 password attempts made, intermittent connection failures observed |
| 15:32:44 | Attack completed — 0 valid passwords found |

## Attack Execution

Brute-force attack executed from Kali against Windows RDP (port 3389) using Hydra 
with the fasttrack.txt wordlist (262 password attempts).

**Command used:** hydra -l usama -P /usr/share/wordlists/fasttrack.txt rdp://192.168.56.10

**Result:** 262 attempts tried over ~2.5 minutes, 0 valid passwords found. Hydra 
experienced repeated intermittent connection failures during the run, consistent 
with known limitations in its experimental RDP module.

<img width="577" height="709" alt="image" src="https://github.com/user-attachments/assets/c5b1bb85-9687-411e-9061-417e98c7b422" />


## Detection Method
**Log source:** Windows Security Event Log, Event ID 4625 (failed logon)

**Query used:**
```
index=main EventCode=4625 Source_Network_Address=192.168.56.20
```

**What it showed:** Failed login events recorded at 07/17/2026 9:32:43 PM, 
originating from source IP 192.168.56.20 (Kali attacker VM), targeting account 
"usama" via network logon (Logon Type 3, NTLM authentication) — consistent with 
the Hydra RDP brute-force attack. Failure reason: "Unknown user name or bad 
password" (Status 0xC000006D).

<img width="821" height="569" alt="image" src="https://github.com/user-attachments/assets/dff633c9-1746-4a7b-b1cc-0dc63d6e7f98" />
<img width="823" height="570" alt="image" src="https://github.com/user-attachments/assets/0ac16a9a-55c4-4b7b-8c38-14d76cab9fe7" />

## Investigation


## Impact Assessment
[What was at risk if this were a real environment — e.g., "Successful brute force would have granted full RDP access to the host, exposing [whatever data/systems the victim VM represented]."]

## Root Cause
[Why the attack was possible — e.g., "No account lockout policy configured, allowing unlimited login attempts."]

## Recommendation
[The specific control that would prevent or catch this faster — e.g., "Implement account lockout after 5 failed attempts; enable MFA on RDP; add a Splunk alert rule for >10 failed logins within 5 minutes from a single source IP."]

## Evidence
![Full attack log](screenshots/attack-log.png)
![Splunk query results](screenshots/splunk-query.png)
