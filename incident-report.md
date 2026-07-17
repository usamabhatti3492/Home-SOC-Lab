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
| 15:32:44 | Attack completed - 0 valid passwords found |

## Attack Execution

Brute-force attack executed from Kali against Windows RDP (port 3389) using Hydra 
with the fasttrack.txt wordlist (262 password attempts).

**Command used:** hydra -l usama -P /usr/share/wordlists/fasttrack.txt rdp://192.168.56.10

**Result:** 262 attempts tried over ~2.5 minutes, 0 valid passwords found. Hydra 
experienced repeated intermittent connection failures during the run, consistent 
with known limitations in its experimental RDP module.

<img width="577" height="709" alt="Hydra brute-force attack execution against RDP" src="https://github.com/user-attachments/assets/c5b1bb85-9687-411e-9061-417e98c7b422" />


## Detection Method
**Log source:** Windows Security Event Log, Event ID 4625 (failed logon)

**Query used:**
```
index=main EventCode=4625 Source_Network_Address=192.168.56.20
```

**What it showed:** Failed login events recorded at 07/17/2026 9:32:43 PM, 
originating from source IP 192.168.56.20 (Kali attacker VM), targeting account 
"usama" via network logon (Logon Type 3, NTLM authentication) - consistent with 
the Hydra RDP brute-force attack. Failure reason: "Unknown user name or bad 
password" (Status 0xC000006D).

<img width="821" height="569" alt="Splunk event log showing failed logon from attack source" src="https://github.com/user-attachments/assets/dff633c9-1746-4a7b-b1cc-0dc63d6e7f98" />
<img width="823" height="570" alt="Failure details confirming source IP and NTLM authentication" src="https://github.com/user-attachments/assets/0ac16a9a-55c4-4b7b-8c38-14d76cab9fe7" />

## Investigation

The failed logon events were reviewed to confirm they matched the Hydra attack 
rather than legitimate access issues. Key indicators confirmed this was the 
brute-force attack:

- **Source IP (192.168.56.20)** matched the Kali attacker VM exactly, not any 
  legitimate host on the network
- **Logon Type 3** (network logon) is consistent with a remote authentication 
  attempt rather than a local console login
- **NTLM authentication package** matched the protocol RDP falls back to when 
  Hydra's connection attempts were made
- All attempts returned the same failure reason ("Unknown user name or bad 
  password"), consistent with a wordlist-based guessing attack rather than a 
  single mistyped login

One additional data point worth noting: during the attack, Hydra's own output 
flagged one attempt (password "Spring2017") as a possible valid credential 
with the account "not active for remote desktop." This was manually verified 
by attempting to log in with that password directly - the login failed, 
confirming this was a false positive from Hydra's experimental RDP module 
rather than an actual valid credential.


## Impact Assessment

No account compromise occurred in this test — the attack failed to guess a 
valid password within the fasttrack.txt wordlist (262 attempts). However, this 
outcome depended entirely on the password not appearing in that specific list. 
Had the account used a weaker or more common password, or had the attack run 
with a larger wordlist (e.g., rockyou.txt), the outcome could have been 
different. In a real environment, a successful brute-force against RDP would 
grant full remote desktop access to the host — equivalent to physical access 
to the machine, including any files, credentials, or lateral network access 
available from that session.
## Root Cause

No account lockout policy was configured on the Windows host, allowing 
unlimited login attempts with no delay, throttling, or blocking after 
repeated failures. This meant the attack was free to attempt all 262 
passwords without any automatic defensive response from the system itself.

## Recommendation

- **Account lockout policy:** Lock the account after 5 failed attempts within 
  a 5-minute window, with a cooldown period before retry is allowed.
- **Multi-factor authentication (MFA):** Require a second factor for RDP 
  access, so a correctly guessed password alone is insufficient to gain access.
- **Splunk alert rule:** Configure an alert to trigger when more than 10 
  EventCode=4625 events occur from a single source IP within 5 minutes — this 
  would have flagged this attack in real time rather than requiring manual 
  log review after the fact.
