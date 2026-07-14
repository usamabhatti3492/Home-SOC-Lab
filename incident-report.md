# Incident Report: Brute-Force RDP Attack

## Summary
[2 sentences, plain language — what happened, what you found]

## Timeline
| Time | Event |
|------|-------|
| 14:02 | First failed login attempt detected |
| 14:03 | 40+ failed attempts from same source IP within 90 seconds |
| 14:04 | [Attack stopped / succeeded / was blocked — whatever actually happened] |

## Detection Method
**Log source:** [e.g., Windows Security Event Log, Event ID 4625]

**Query used:**
```
index=main EventCode=4625 | stats count by src_ip
```

**What it showed:** [Explain what the query returned and how it pointed you to the attack]

## Investigation
[Walk through your reasoning step by step — what you looked at first, what you ruled out, how you confirmed it was a brute-force attempt and not something else. This section is where you demonstrate analyst thinking, not just tool output.<img width="1899" height="1005" alt="image" src="https://github.com/user-attachments/assets/6f3a5ed9-bda2-46c6-a11c-372527c4042a" />
]

## Impact Assessment
[What was at risk if this were a real environment — e.g., "Successful brute force would have granted full RDP access to the host, exposing [whatever data/systems the victim VM represented]."]

## Root Cause
[Why the attack was possible — e.g., "No account lockout policy configured, allowing unlimited login attempts."]

## Recommendation
[The specific control that would prevent or catch this faster — e.g., "Implement account lockout after 5 failed attempts; enable MFA on RDP; add a Splunk alert rule for >10 failed logins within 5 minutes from a single source IP."]

## Evidence
![Full attack log](screenshots/attack-log.png)
![Splunk query results](screenshots/splunk-query.png)
