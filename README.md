# networkScan2020

A simple network scanner built with Scapy for Python

## Description

This project is a command-line network scanning tool that allows users to discover devices on a local network and identify open ports. It uses the Scapy library to send ARP requests for network discovery and TCP SYN packets for port scanning. Ideal for network administrators and security enthusiasts who need quick network reconnaissance capabilities.

## Features

- ARP scanning to discover devices on a local network and map IP addresses to MAC addresses
- TCP port scanning using SYN packets to identify open ports
- Support for scanning individual ports or a range of ports
- Command-line interface with helpful usage instructions

## Technologies Used

- Python 3
- Scapy (packet manipulation library)
- argparse (command-line argument parsing)

## Installation

```bash
# Clone the repository
git clone https://github.com/bryanseah234/networkScan2020.git

# Navigate to project directory
cd networkScan2020

# Install dependencies
pip install scapy
```

## Usage

```bash
# ARP Scan - Discover devices on a local network
sudo python3 scanner.py ARP 192.168.1.1/24

# TCP Scan - Scan specific ports
sudo python3 scanner.py TCP 192.168.1.1 22 80 443

# TCP Scan - Scan a range of ports
sudo python3 scanner.py TCP 192.168.1.1 --range 0 1000
```

**Note:** On UNIX-based systems, run the script as root (use `sudo`) to allow raw packet operations.

### ARP Scan
Sends ARP requests to all devices on the local network and collects the ARP replies to discover IP address to MAC address mappings. Either an IP address or IP address range can be used.

### TCP Scan
Sends TCP SYN packets to specified ports and collects the SYN+ACK replies to discover which ports are open. Either specific ports or a range of ports can be scanned.

## Disclaimer

1. FOR EDUCATIONAL PURPOSES ONLY
2. USE AT YOUR OWN DISCRETION

## License

MIT License

---

**Author:** <a href="https://github.com/bryanseah234">bryanseah234</a>
