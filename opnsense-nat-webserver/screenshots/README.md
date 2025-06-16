# Screenshots Documentation

This directory contains visual documentation of the complete OPNsense NAT Web Server implementation, organized by project phase and component.

## VMware Configuration (`vmware-config/`)

### Virtual Machine Settings
- `opnsense-vm-memory.JPG` - OPNsense VM memory allocation settings
- `opnsense-vm-processors.JPG` - OPNsense VM processor configuration
- `opnsense-vm-disk.JPG` - OPNsense VM hard disk settings
- `opnsense-vm-iso.JPG` - OPNsense VM CD/DVD with ISO file
- `opnsense-vm-network1.JPG` - OPNsense VM Network Adapter 1 (VMnet0)
- `opnsense-vm-network2.JPG` - OPNsense VM Network Adapter 2 (VMnet1)
- `webserver-vm-memory.JPG` - Web server VM memory settings
- `webserver-vm-processors.JPG` - Web server VM processor settings
- `webserver-vm-disk.JPG` - Web server VM hard disk settings
- `webserver-vm-iso.JPG` - Web server VM CD/DVD with Ubuntu ISO
- `webserver-vm-network.JPG` - Web server VM network adapter (VMnet1)

### Virtual Network Setup
- `vmware-vmnet1-settings.JPG` - VMware Virtual Network Editor VMnet1 configuration
- `vmware-vmnet0-settings.JPG` - VMware Virtual Network Editor VMnet0 configuration

## OPNsense Configuration (`opnsense-interface/`)

### Web Interface and Dashboard
- `opnsense-dashboard.JPG` - Main dashboard showing system status and interface statistics
- `opnsense-console-terminal.JPG` - Console interface showing LAN/WAN IP assignments

### Interface Configuration
- `opnsense-interface-assignments.JPG` - WAN and LAN interface device assignments
- `opnsense-wan-interface.JPG` - WAN interface DHCP client configuration
- `opnsense-lan-interface.JPG` - LAN interface static IP configuration (192.168.10.2/24)

### NAT and Firewall Rules
- `opnsense-nat-outbound.JPG` - Automatic outbound NAT rule configuration
- `opnsense-nat-port-forward.JPG` - HTTP/HTTPS port forwarding rules to web server
- `opnsense-firewall-wan-rules.JPG` - WAN firewall rules allowing external access
- `opnsense-dhcp-config.JPG` - DHCP server configuration for internal network

## Web Server Configuration (`webserver-config/`)

### System Configuration
- `webserver-ip-config.JPG` - Network interface configuration (ip addr show)
- `webserver-routing-table.JPG` - Default gateway configuration (ip route show)
- `webserver-netplan-config.JPG` - Ubuntu netplan network configuration file

### Apache Web Server
- `webserver-apache-status.JPG` - Apache service status and process information
- `webserver-port-listening.JPG` - Ports 80 and 443 listening status

## Testing and Validation (`testing-results/`)

### HTTP Testing (Successful)
- `test-http-internal-access.JPG` - Browser accessing web server via 192.168.10.103
- `test-http-external-access.JPG` - Browser accessing web server via OPNsense WAN IP
- `test-curl-http-success.JPG` - Terminal curl commands showing successful HTTP access

### HTTPS Testing (SSL Limitations)
- `test-curl-https-ssl-errors.JPG` - Terminal curl showing SSL configuration errors
- `test-https-external-failure.JPG` - Browser HTTPS failure via external IP
- `test-https-internal-failure.JPG` - Browser HTTPS failure via internal IP

### Network Connectivity
- `test-traceroute.JPG` - Traceroute to 8.8.8.8 showing internet connectivity through OPNsense

## Key Findings

### Successful Implementation
- ✅ **NAT Port Forwarding**: HTTP traffic successfully routed from external network to internal web server
- ✅ **Network Segmentation**: Clear separation between external (192.168.1.0/24) and internal (192.168.10.0/24) networks
- ✅ **DHCP Services**: Automatic IP assignment working correctly
- ✅ **Internet Connectivity**: Internal server accessing internet via OPNsense gateway

### Expected Limitations
- ⚠️ **HTTPS Functionality**: Limited by default Apache SSL configuration (certificate and SSL module setup required)
- ⚠️ **SSL Certificate**: Self-signed or missing certificates causing browser/curl SSL errors

This documentation provides complete visual proof of a working NAT gateway implementation with proper network security and controlled external access.
