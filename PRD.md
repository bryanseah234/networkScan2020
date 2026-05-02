# PRD: networkScan2020

## Overview
A Python CLI tool for local network discovery and port scanning, built on Scapy. Supports two scan modes: ARP scanning (discover devices on a subnet) and TCP SYN scanning (find open ports on a target). Intended for network administrators or students learning network security concepts.

## Goals
- Perform ARP host discovery on a subnet (returns IP → MAC mappings)
- Perform TCP SYN port scanning on individual hosts
- Accept IP addresses, CIDR ranges, and hostnames as targets
- Provide a clean CLI via argparse

## Non-Goals
- GUI or web interface
- UDP/ICMP scanning
- OS fingerprinting
- Service version detection
- Output to file formats (JSON, XML, CSV)
- Parallel scanning / multi-threading

## User Stories
- As a sysadmin, I want to discover all devices on 192.168.1.0/24 so I can audit what's connected.
- As a security student, I want to scan which ports are open on a target IP to practice enumeration.
- As a network engineer, I want to see the MAC address of every host on my LAN segment.

## Tech Stack
- **Language**: Python 3.x
- **Libraries**: `scapy` (pip), `argparse` (stdlib), `socket` (stdlib)
- **Privilege**: requires root/Administrator (Scapy needs raw socket access)

## Architecture
```
networkScan2020/
├── scanner.py      # CLI entry point + two scan functions
└── requirements.txt
```

**Functions:**
- `arp_scan(ip)` → list of `{'IP': str, 'MAC': str}`
- `tcp_scan(ip, ports)` → list of open port numbers
- `main()` → argparse subcommand router

## Features (detailed)

### ARP Scan (`ARP` subcommand)
- Sends Ethernet broadcast + ARP request to target IP or CIDR range
- Uses `Ether(dst="ff:ff:ff:ff:ff:ff") / ARP(pdst=ip)`
- Collects replies via `srp()` with 2-second timeout, 1 retry
- **Output**: prints `IP ==> MAC` for each responding host
- **Acceptance**: correctly identifies all reachable hosts on the subnet

### TCP SYN Scan (`TCP` subcommand)
- Sends SYN packets to specified port(s) using `IP / TCP(flags="S")`
- Ports can be a list of individual ports or a range (via `--range` flag)
- Checks for SYN-ACK response (`flags == "SA"`) to determine open ports
- **Output**: prints `Port N is open.` for each open port
- **Acceptance**: correctly identifies open ports (e.g., 80, 443 on a web server)

### CLI Interface
- `python scanner.py ARP <ip_or_cidr>` — ARP scan
- `python scanner.py TCP <ip> <port1> [port2 ...]` — TCP scan individual ports
- `python scanner.py TCP <ip> <low> <high> --range` — TCP scan port range
- Raises `ValueError` with message if hostname cannot be resolved

## Data / Config
No configuration files. All inputs passed as CLI arguments.

## Deployment / Run
```bash
pip install scapy
# Linux/Mac: run with sudo
sudo python scanner.py ARP 192.168.1.1/24
sudo python scanner.py TCP 192.168.1.1 80 443 8080
sudo python scanner.py TCP 192.168.1.1 1 1000 --range
# Windows: run from admin PowerShell
```

## Constraints & Notes
- **Root required**: Scapy requires raw socket access; must run as root on Linux/Mac or Administrator on Windows
- **Firewall**: firewalls can block ARP replies or drop SYN packets, causing false negatives
- **Legal**: scanning networks you don't own or have explicit permission to test is illegal in most jurisdictions
- **Timeout**: 2 seconds per scan; large ranges may take a while
- **Scapy version**: tested with Scapy 2.x; API may differ on older versions
