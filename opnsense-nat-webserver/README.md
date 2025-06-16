# OPNsense NAT Web Server

## Project Overview

This project demonstrates the implementation of a NAT gateway solution using OPNsense firewall to provide external access to an internal web server. Built in a VMware virtualised environment, it showcases network segmentation, NAT configuration, and firewall management skills.

## What I Built

- **OPNsense VM**: Configured as NAT gateway with dual network interfaces
- **Ubuntu Server VM**: Internal web server running Apache
- **VMware Network Setup**: Separate virtual networks for WAN and LAN traffic
- **Port Forwarding**: HTTP/HTTPS access from external network to internal server
- **DHCP Services**: Automatic IP assignment for internal network devices

## Technical Implementation

**Network Architecture:**
- External Network: 192.168.1.0/24 (via VMware bridged connection)
- Internal Network: 192.168.10.0/24 (via VMware host-only network)
- OPNsense WAN: DHCP client on external network
- OPNsense LAN: Static IP 192.168.10.2/24
- Web Server: DHCP-assigned 192.168.10.103/24

**Key Configurations:**
- NAT port forwarding for HTTP (80) and HTTPS (443)
- Automatic outbound NAT for internal network internet access
- DHCP server providing IP range 192.168.10.100-200
- Firewall rules allowing external web traffic

## Testing Results

✅ **Internal Access**: Web server accessible via 192.168.10.103  
✅ **External Access**: Web server accessible via OPNsense WAN IP  
✅ **Internet Connectivity**: Internal network routing through OPNsense confirmed  
✅ **NAT Functionality**: Port forwarding working for HTTP traffic  
⚠️ **HTTPS**: Limited by default SSL configuration (expected for test environment)

## Challenges Overcome

- **IP Subnet Conflicts**: Initially configured VMs on same subnet as host network, causing routing issues
- **Network Adapter Configuration**: Required reconfiguration of VMware network adapters and OPNsense interfaces
- **Access Issues**: Resolved connectivity problems between host and OPNsense management interface

## Technologies Used

- **Virtualisation**: VMware Workstation
- **Firewall**: OPNsense 25.1
- **Operating System**: Ubuntu Server 24.04.2 LTS
- **Web Server**: Apache HTTP Server
- **Network Configuration**: Netplan (Ubuntu), VMware Virtual Networks

## Project Structure

- `documentation/` - Network design and implementation process
- `configs/` - Detailed configuration settings for all components
- `screenshots/` - Visual documentation of configurations and testing

## Skills Demonstrated

- Virtual network design and implementation
- Firewall configuration and management
- NAT and port forwarding setup
- Linux server administration
- Network troubleshooting and problem resolution
- Technical documentation and testing validation

---

This project represents a practical implementation of enterprise networking concepts in a controlled virtual environment, demonstrating hands-on experience with network security and infrastructure management.
