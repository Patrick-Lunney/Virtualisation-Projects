# Implementation Process

## Project Setup

### Initial Planning
- **Objective**: Design and deploy a virtualised network infrastructure demonstrating enterprise firewall concepts - specifically NAT gateway functionality, DHCP services, port forwarding, and network segmentation using OPNsense to bridge external (192.168.1.0/24) and internal (192.168.10.0/24) networks
- **Environment**: VMware Workstation on Windows host
- **Target Architecture**: Dual-homed firewall with network segmentation

### Software Requirements
- **VMware Workstation**: Host virtualisation platform
- **OPNsense ISO**: OPNsense-25.1-dvd-amd64.iso
- **Ubuntu Server ISO**: ubuntu-24.04.2-live-server-amd64.iso

## Step 1: VMware Network Configuration

### Virtual Network Setup
1. **VMnet0 Configuration**: Set to Bridged mode for WAN connectivity
2. **VMnet1 Configuration**: Set to Host-only for internal LAN
3. **VMnet1 DHCP**: Disabled VMware DHCP service (OPNsense will handle this)
4. **IP Planning**: Selected 192.168.10.0/24 to avoid conflicts with host network

### Network Adapter Assignment
- **OPNsense VM**: Two adapters (VMnet0 for WAN, VMnet1 for LAN)
- **Web Server VM**: Single adapter (VMnet1 for internal connectivity only)

## Step 2: OPNsense Installation and Configuration

### VM Creation
- **Hardware**: 2GB RAM, 2 CPU cores, 20GB disk
- **Boot**: From OPNsense ISO
- **Installation**: Standard OPNsense installation to disk

### Initial Interface Configuration
1. **Console Setup**: Assigned em0 as WAN, em1 as LAN via console menu
2. **LAN IP Assignment**: Set 192.168.10.2/24 as static IP
3. **WAN Configuration**: Configured as DHCP client
4. **Web Interface Access**: Accessed via https://192.168.10.2

### Core Service Configuration
1. **DHCP Server**: Enabled on LAN interface with range 192.168.10.100-200
2. **NAT Rules**: Configured automatic outbound NAT
3. **Port Forwarding**: Added HTTP (80) and HTTPS (443) rules to 192.168.10.103
4. **Firewall Rules**: Applied automatic rules from port forwarding configuration

## Step 3: Web Server Deployment

### Ubuntu Server Installation
- **VM Creation**: 4GB RAM, 2 CPU cores, 20GB disk
- **Network**: Single adapter on VMnet1
- **Installation**: Standard Ubuntu Server 24.04.2 installation
- **User Setup**: Created admin_patrick user account

### Network Configuration
1. **IP Assignment**: Received 192.168.10.103 via DHCP from OPNsense
2. **Gateway Verification**: Confirmed 192.168.10.2 as default route
3. **DNS Configuration**: Set to use OPNsense (192.168.10.2) and Google DNS (8.8.8.8)

### Web Server Setup
1. **Apache Installation**:
```bash
  sudo apt update
  sudo apt install apache2
```

3. **Service Verification**: Confirmed apache2 service running on ports 80 and 443
4. **Content Customisation**: Modified default page to "OPNsense NAT Test Page"

## Step 4: Testing and Validation

### Internal Connectivity Testing
Commands used:
```bash
  curl http://localhost
  curl http://192.168.10.103
  traceroute 8.8.8.8
```

**Results**: All internal connectivity working correctly

### External Access Testing
1. **HTTP Access**: Successfully accessed web server via OPNsense WAN IP
2. **Browser Testing**: Confirmed "OPNsense NAT Test Page" accessible externally
3. **NAT Verification**: Port forwarding working for HTTP traffic

## Challenges Encountered and Solutions

### Issue 1: IP Subnet Conflicts
- **Problem**: Initially configured VMnet1 to use same subnet as host network (192.168.1.0/24)
- **Impact**: Routing confusion - traffic went to host router instead of OPNsense
- **Solution**: Reconfigured VMnet1 to use 192.168.10.0/24 subnet
- **Lesson**: Always use separate subnets for internal networks

### Issue 2: VMware Network Adapter Configuration
- **Problem**: Unable to access OPNsense web interface from host machine
- **Impact**: Could not complete initial configuration
- **Solution**: 
  - Verified VMware virtual network adapter settings
  - Reconfigured OPNsense LAN interface settings
  - Ensured host had proper network adapter for VMnet1
- **Lesson**: Verify all network connectivity before proceeding with configuration

### Issue 3: Interface Assignment Confusion
- **Problem**: Initially mixed up WAN and LAN interface assignments
- **Impact**: No internet connectivity and management access issues
- **Solution**: Used OPNsense console to reassign interfaces correctly
- **Lesson**: Document interface assignments during initial setup

## Final Configuration Summary

### Network Topology Achieved
- **External Network**: 192.168.1.0/24 (host network)
- **OPNsense WAN**: 192.168.1.119 (DHCP from host router)
- **Internal Network**: 192.168.10.0/24 (isolated via OPNsense)
- **OPNsense LAN**: 192.168.10.2 (static gateway)
- **Web Server**: 192.168.10.103 (DHCP from OPNsense)

### Functional Capabilities
- ✅ **Internal server internet access** via NAT through OPNsense
- ✅ **External HTTP access** to internal web server via port forwarding
- ✅ **Network segmentation** between external and internal networks
- ✅ **DHCP services** for internal network device management
- ✅ **Firewall protection** with controlled access rules

This implementation successfully demonstrates enterprise-level network concepts in a virtualised environment, showcasing practical skills in firewall configuration, network design, and troubleshooting.
