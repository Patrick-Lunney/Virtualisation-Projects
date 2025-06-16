# OPNsense Configuration

## VM Hardware Specifications

- **Memory**: 2 GB RAM
- **Processors**: 2 cores
- **Hard Disk**: 20 GB (SCSI)
- **Network Adapters**: 2 (em0 for WAN, em1 for LAN)
- **ISO**: OPNsense-25.1-dvd-amd64.iso

## Interface Configuration

### Interface Assignments
- **WAN Interface**: em0 (00:0c:29:67:19:25) - VMware Network Adapter 1
- **LAN Interface**: em1 (00:0c:29:67:19:2f) - VMware Network Adapter 2

### WAN Interface (em0)
- **Configuration Type**: DHCP Client
- **External Connection**: VMnet0 (Bridged)
- **Current IP**: 192.168.1.119/24 (dynamically assigned)
- **Gateway**: 192.168.1.1 (host router)

### LAN Interface (em1)
- **Configuration Type**: Static IPv4
- **IP Address**: 192.168.10.2/24
- **External Connection**: VMnet1 (Host-only)
- **Purpose**: Internal network gateway

## DHCP Server Configuration

### LAN DHCP Settings
- **Status**: Enabled on LAN interface
- **Subnet**: 192.168.10.0
- **Subnet Mask**: 255.255.255.0
- **Available Range**: 192.168.10.1 - 192.168.10.254
- **DHCP Pool**: 192.168.10.100 - 192.168.10.200
- **Gateway**: 192.168.10.2 (OPNsense LAN interface)

## NAT Configuration

### Outbound NAT
- **Mode**: Automatic outbound NAT rule generation
- **Rules**: Auto-created for LAN networks, Loopback networks, 127.0.0.0/8
- **Interface**: WAN
- **Translation**: LAN traffic (192.168.10.0/24) to WAN IP

### Port Forwarding Rules
| Interface | Protocol | Source | Destination Port | Target IP | Target Port | Description |
|-----------|----------|---------|------------------|-----------|-------------|-------------|
| WAN | TCP | Any | 80 (HTTP) | 192.168.10.103 | 80 (HTTP) | HTTP to Web Server |
| WAN | TCP | Any | 443 (HTTPS) | 192.168.10.103 | 443 (HTTPS) | HTTPS to Web Server |
| LAN | TCP | Any | 80, 443 | LAN address | * | Anti-Lockout Rule |

## Firewall Rules

### WAN Rules
| Protocol | Source | Port | Destination | Port | Description |
|----------|---------|------|-------------|------|-------------|
| IPv4 TCP | Any | Any | 192.168.10.103 | 80 (HTTP) | HTTP to Web Server |
| IPv4 TCP | Any | Any | 192.168.10.103 | 443 (HTTPS) | HTTPS to Web Server |
| IPv4 TCP | Any | Any | WAN address | 80 (HTTP) | Allow HTTP to Web Server |
| IPv4 TCP | Any | Any | WAN address | 443 (HTTPS) | Allow HTTPS to Web Server |

### LAN Rules
- **Default**: Allow all outbound traffic from LAN to any destination
- **Anti-lockout**: Automatic rule preventing lockout from management interface

All configurations were applied through the OPNsense web interface at https://192.168.10.2/ui/core/dashboard using default administrative credentials.
