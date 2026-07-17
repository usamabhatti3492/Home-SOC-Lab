# Lab Environment

## Virtualization
- VirtualBox 7.x

## Virtual Machines
| VM | Role | OS |
|----|------|-----|
| Kali Linux 2026.1 | Attacker | Debian-based |
| Windows 10 Evaluation | Victim | Windows 10 Pro |

## Network Configuration
- Both VMs connected via VirtualBox **Internal Network** (`intnet`)
- Static IPs assigned: Kali `192.168.56.20`, Windows `192.168.56.10`
- No default gateway — fully isolated from host network and internet
- No internet access during attack simulation (isolated by design)
- Verified connectivity via `ping` between hosts before running any tests
<img width="1110" height="595" alt="image" src="https://github.com/user-attachments/assets/278cd49b-f5b1-4496-b297-c49b6b32b390" />



## SIEM
- Splunk Free, ingesting Windows Security Event Logs from the victim VM
<img width="700" height="465" alt="image" src="https://github.com/user-attachments/assets/6c17c938-8b4c-4616-afe5-782d83fc19c1" />

## Splunk Configuration
- Local Event Log Collection configured to ingest Windows Security event log
- Destination index: main
<img width="700" height="465" alt="image" src="https://github.com/user-attachments/assets/759db502-8f9a-46f5-b57a-8d7e9bd29ca7" />

- Verified data ingestion (1,333 events indexed from Windows Security log):

  
<img width="700" height="465" alt="image" src="https://github.com/user-attachments/assets/d0cd1d0b-fafa-465f-857a-254ee1a576b5" />

## Tools Used for Simulation
- Hydra (brute-force attack tool, run from Kali)

## Notes
- Static IPs reset when switching adapter mode between NAT and Internal Network (needed for Splunk download); reapplied manually after each switch using `sudo ip addr add 192.168.56.10/24 dev eth0` on Kali and via Network Connections settings on Windows
- Splunk index field defaulted to "default" explicitly set to "main" to match search queries used later
- Windows Firewall and adapter network-name mismatches were checked as part of troubleshooting a temporary "100% packet loss" connectivity issue, resolved by confirming both VMs were on the same Internal Network name
