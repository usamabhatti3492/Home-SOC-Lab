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
<img width="1916" height="1012" alt="image" src="https://github.com/user-attachments/assets/bf412698-4daf-40e8-a6d3-ff19608efa34" />





## SIEM
- Splunk Free, ingesting Windows Security Event Logs from the victim VM
<img width="700" height="465" alt="image" src="https://github.com/user-attachments/assets/ee450c82-fc9e-4368-8821-f49343ba1e3a" />


## Splunk Configuration
- Local Event Log Collection configured to ingest Windows Security event log
- Destination index: main
<img width="700" height="465" alt="image" src="https://github.com/user-attachments/assets/5c5f4eb0-80b9-4eb5-92c9-7f1fc7cf3537" />


- Verified data ingestion (1,092 events indexed from Windows Security log):

  
<img width="700" height="465" alt="image" src="https://github.com/user-attachments/assets/47a3806b-1cea-4710-bb5c-0876a03a1090" />


## Tools Used for Simulation
- Hydra (brute-force attack tool, run from Kali)

## Notes
- Static IPs reset when switching adapter mode between NAT and Internal Network (needed for Splunk download); reapplied manually after each switch using `sudo ip addr add 192.168.56.10/24 dev eth0` on Kali and via Network Connections settings on Windows
- Splunk index field defaulted to "default" explicitly set to "main" to match search queries used later
- Windows Firewall and adapter network-name mismatches were checked as part of troubleshooting a temporary "100% packet loss" connectivity issue, resolved by confirming both VMs were on the same Internal Network name
