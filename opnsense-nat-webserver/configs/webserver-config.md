# Web Server Configuration

## VM Hardware Specifications

- **Memory**: 4 GB RAM
- **Processors**: 2 cores
- **Hard Disk**: 20 GB (SCSI)
- **Network Adapter**: 1 (VMnet1 Host-only)
- **ISO**: ubuntu-24.04.2-live-server-amd64.iso

## Operating System

- **Distribution**: Ubuntu Server 24.04.2 LTS
- **Installation**: Standard server installation
- **User**: admin_patrick
- **Interface**: Command-line only (no GUI)

## Network Configuration

### Interface Settings
- **Interface**: ens33 (VMware network adapter)
- **IP Address**: 192.168.10.103/24 (DHCP assigned)
- **Gateway**: 192.168.10.2 (OPNsense LAN interface)
- **DNS Servers**: 192.168.10.2, 8.8.8.8

### Netplan Configuration
Configuration file: /etc/netplan/50-cloud-init.yaml

    network:
      version: 2
      ethernets:
        ens33:
          addresses:
            - "192.168.10.103/24"
          nameservers:
            addresses:
              - 192.168.10.2
              - 8.8.8.8
          routes:
            - to: "default"
              via: "192.168.10.2"

## Web Server Setup

### Apache Installation
Commands used:
```bash
sudo apt update
sudo apt install apache2
```

### Service Status
- **Service**: apache2.service (The Apache HTTP Server)
- **Status**: Active (running)
- **Process ID**: 2730
- **Memory Usage**: ~7.7M
- **Listening Ports**: 80 (HTTP), 443 (HTTPS)

### Default Configuration
- **Document Root**: /var/www/html
- **Default Page**: Apache default "It works!" page replaced with custom "OPNsense NAT Test Page"
- **HTTP**: Port 80 (enabled)
- **HTTPS**: Port 443 (default configuration, limited SSL functionality)

## Network Connectivity

### Routing Table
- **Default Route**: 192.168.10.2 (OPNsense gateway)
- **Local Network**: 192.168.10.0/24 via ens33
- **Internet Access**: Via NAT through OPNsense

### Connectivity Testing
Commands used:
```bash
curl http://localhost
curl http://192.168.10.103
traceroute 8.8.8.8
```

**Results**: 
- Local HTTP access working
- Internet connectivity confirmed via OPNsense gateway
- External access via NAT port forwarding functional

All configurations use standard Ubuntu Server practices with minimal customisation for testing purposes.
