# Network Design

## Overview

This project implements a dual-homed firewall architecture using VMware virtual networks to simulate a real-world enterprise environment where internal servers need controlled external access.

## Network Topology

**External Network (192.168.1.0/24)**
- Host router/internet connection
- VMnet0 (Bridged) connects to:

**OPNsense Firewall**
- WAN Interface: DHCP client (receives IP from 192.168.1.x range)
- LAN Interface: Static IP 192.168.10.2/24
- VMnet1 (Host-only) connects to:

**Internal Network (192.168.10.0/24)**
- Ubuntu Web Server: 192.168.10.103/24 (DHCP assigned)

## VMware Network Configuration

### VMnet0 (WAN/External)
- **Type**: Bridged connection
- **Purpose**: Connects OPNsense WAN interface to host network
- **IP Range**: 192.168.1.0/24 (host network subnet)
- **DHCP**: Provided by host router

### VMnet1 (LAN/Internal)
- **Type**: Host-only network
- **Subnet**: 192.168.10.0/24
- **Subnet Mask**: 255.255.255.0
- **DHCP**: Disabled (OPNsense provides DHCP services)
- **Purpose**: Isolated internal network for servers

## Design Rationale

### Network Separation
- **External traffic** isolated from internal network
- **Internal servers** cannot directly access internet
- **All external access** must go through OPNsense firewall

### IP Address Planning
- **192.168.1.0/24**: External network (existing home/office network)
- **192.168.10.0/24**: Internal network (chosen to avoid conflicts)
- **DHCP Range**: 192.168.10.100-200 (allows for static assignments below .100)

### Security Benefits
- **Default deny**: Internal network has no direct internet access
- **Controlled access**: Only explicitly allowed services (HTTP/HTTPS) forwarded
- **Network segmentation**: Clear separation between trusted and untrusted networks
- **Centralized logging**: All traffic passes through OPNsense for monitoring

## Traffic Flow

### Outbound (Internal to Internet)
1. Web server (192.168.10.103) sends traffic to default gateway (192.168.10.2)
2. OPNsense performs NAT translation
3. Traffic exits via WAN interface to internet

### Inbound (Internet to Internal)
1. External client connects to OPNsense WAN IP
2. Port forwarding rules direct HTTP/HTTPS to 192.168.10.103
3. OPNsense performs reverse NAT translation
4. Traffic reaches web server on internal network

This design mirrors real-world enterprise networks where DMZ servers require controlled external access while maintaining security through network segmentation.
