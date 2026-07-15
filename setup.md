# Lab Environment

## Virtualization
- VirtualBox 7.x

## Virtual Machines
| VM | Role | OS |
|----|------|-----|
| Kali Linux 2026.1 | Attacker | Debian-based |
| Windows 10 Evaluation | Victim | Windows 10 |

## Network Configuration
- Both VMs connected via VirtualBox **Internal Network** (`intnet`)
- Static IPs assigned: Kali `192.168.56.20`, Windows `192.168.56.10`
- No default gateway — fully isolated from host network and internet
- No internet access during attack simulation (isolated by design)
- Verified connectivity via `ping` between hosts before running any tests
<img width="1910" height="995" alt="image" src="https://github.com/user-attachments/assets/278cd49b-f5b1-4496-b297-c49b6b32b390" />



## SIEM
- Splunk Free, ingesting Windows Security Event Logs from the victim VM

## Tools Used for Simulation
- Hydra (brute-force attack tool, run from Kali)

## Notes
- Snapshots taken before each major test to allow quick rollback
- [Add any other config detail worth noting — e.g., specific Windows audit policy settings enabled to generate Event ID 4625 logs]
