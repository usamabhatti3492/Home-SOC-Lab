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
- No internet access during attack simulation (isolated by design)
- Verified connectivity via `ping` between hosts before running any tests

## SIEM
- Splunk Free, ingesting Windows Security Event Logs from the victim VM

## Tools Used for Simulation
- Hydra (brute-force attack tool, run from Kali)

## Notes
- Snapshots taken before each major test to allow quick rollback
- [Add any other config detail worth noting — e.g., specific Windows audit policy settings enabled to generate Event ID 4625 logs]
