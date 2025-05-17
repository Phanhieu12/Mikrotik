# MikroTik RouterOS SSH Commands Guide

This repository provides a comprehensive guide to retrieving configuration and status information from MikroTik RouterOS (ROS) using SSH commands. It is designed for network administrators, MikroTik technicians, and enthusiasts managing MikroTik devices. The commands are categorized by system components, covering everything from system resources to advanced routing and monitoring.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [System Information](#system-information)
4. [Network Configuration](#network-configuration)
5. [Firewall Configuration](#firewall-configuration)
6. [Wireless Configuration](#wireless-configuration)
7. [VPN Configuration](#vpn-configuration)
8. [User Management](#user-management)
9. [Quality of Service (QoS)](#quality-of-service-qos)
10. [Routing Configuration](#routing-configuration)
11. [System Services](#system-services)
12. [Monitoring and Logging](#monitoring-and-logging)
13. [Scripts and Scheduler](#scripts-and-scheduler)
14. [Miscellaneous Configurations](#miscellaneous-configurations)
15. [Export Full Configuration](#export-full-configuration)
16. [Real-Time Monitoring](#real-time-monitoring)
17. [Sample Script for Collecting Information](#sample-script-for-collecting-information)
18. [Practical Examples](#practical-examples)
19. [FAQ](#faq)
20. [Tips and Best Practices](#tips-and-best-practices)
21. [References](#references)

## Introduction
MikroTik RouterOS (ROS) is a powerful operating system for networking devices, offering extensive configuration options via its Command Line Interface (CLI) over SSH. This guide lists commands to retrieve detailed system information, organized by functional areas, to assist in monitoring, troubleshooting, and managing MikroTik devices.

## Prerequisites
Before using the commands, ensure the following:
- **SSH Access**: Enable SSH on the MikroTik device:
  ```bash
  /ip service set ssh port=22 disabled=no
  ```
- **Client Tools**: Use SSH clients like:
  - **Windows**: PuTTY or Windows Terminal.
  - **Linux/macOS**: Terminal with `ssh` command:
    ```bash
    ssh admin@<IP_ADDRESS>
    ```
- **Admin Privileges**: Ensure the user account has full administrative rights.
- **File Transfer**: Use SCP or FTP to retrieve exported files:
  ```bash
  scp admin@<IP_ADDRESS>:/<filename>.rsc .
  ```
- **Backup**: Always back up your configuration before making changes:
  ```bash
  /system backup save name=backup
  ```

## System Information
Retrieve details about hardware, software, and system status:
```bash
/system resource print
# Example Output:
#  uptime: 5d12h34m56s
#  version: 7.12 (stable)
/system routerboard print
/system license print
/system health print
/system identity print
/system clock print
/system ntp client print
/system history print
/system package print
```

## Network Configuration
Inspect interfaces, IP settings, and related configurations:
```bash
/interface print detail
/interface monitor-traffic [find]
/ip address print detail
# Example Output:
#  0   address=192.168.1.1/24 network=192.168.1.0 interface=ether1
/ip route print detail
/ip arp print
/ip dns print
/ip dhcp-client print detail
/ip dhcp-server print detail
/ip dhcp-server lease print
/ipv6 address print detail
/ipv6 route print detail
/ipv6 nd print
/interface vlan print detail
/interface bridge print detail
/interface bridge port print
/interface bridge nat print
/interface pppoe-client print detail
/interface pppoe-server server print
/interface ovpn-client print detail
/interface ovpn-server server print
/ip ipsec policy print
/ip ipsec peer print
```

## Firewall Configuration
Review firewall rules, NAT, and mangle settings:
```bash
/ip firewall filter print detail
/ip firewall nat print detail
# Example Output:
#  0 chain=dstnat action=dst-nat to-addresses=192.168.1.100 dst-port=80 protocol=tcp
/ip firewall mangle print detail
/ip firewall raw print detail
/ip firewall service-port print
```

## Wireless Configuration
For devices with wireless capabilities:
```bash
/interface wireless print detail
/interface wireless registration-table print
/interface wireless access-list print
/interface wireless security-profiles print detail
```

## VPN Configuration
Inspect VPN settings (PPTP, L2TP, IPSec):
```bash
/interface pptp-server server print
/interface pptp-client print detail
/interface l2tp-server server print
/interface l2tp-client print detail
/ip ipsec installed-sa print
/ip ipsec policy print
/ip ipsec peer print
```

## User Management
Manage users and authentication:
```bash
/user print detail
/user active print
/radius print
/user aaa print
```

## Quality of Service (QoS)
Review bandwidth management configurations:
```bash
/queue simple print detail
/queue tree print detail
/queue type print
```

## Routing Configuration
Inspect static and dynamic routing:
```bash
/ip route print detail
/routing ospf instance print detail
/routing ospf neighbor print
/routing bgp peer print detail
/routing rip print
```

## System Services
Check service and proxy settings:
```bash
/ip service print
/ip proxy print
/ip proxy access print
/ip socks print
/ip upnp print
/ip upnp interfaces print
```

## Monitoring and Logging
Monitor logs and network activity:
```bash
/log print
# Example Output:
#  time=12:34:56 topic=system,info message="user admin logged in via ssh"
/tool netwatch print
/tool traffic-monitor print
/tool sniffer quick
```

## Scripts and Scheduler
List automation scripts and schedules:
```bash
/system script print detail
/system scheduler print detail
```

## Miscellaneous Configurations
Additional settings:
```bash
/snmp print
/snmp community print
/system ntp client print
/system ntp server print
/system watchdog print
```

## Export Full Configuration
Save the entire configuration to a file:
```bash
/export verbose file=full-config
```
Retrieve the file:
```bash
scp admin@<IP_ADDRESS>:/full-config.rsc .
```

## Real-Time Monitoring
Monitor system and network activity in real-time:
```bash
/interface monitor-traffic [find]
/ip firewall connection print
/ping <IP_ADDRESS> count=5
/tool traceroute <IP_ADDRESS>
/tool bandwidth-test <IP_ADDRESS>
/system resource monitor
```

## Sample Script for Collecting Information
A script to collect and save system information:
```bash
/system script
add name=collect-system-info source={
  :local output "";
  :set output ($output . [/system identity get name] . "\n");
  :set output ($output . [/system resource print as-value] . "\n");
  :set output ($output . [/ip address print as-value] . "\n");
  :set output ($output . [/interface print detail as-value] . "\n");
  :set output ($output . [/ip firewall filter print as-value] . "\n");
  :set output ($output . [/ip route print as-value] . "\n");
  /file print file=system-info.txt value=$output;
}
```
Run the script:
```bash
/system script run collect-system-info
```

## Practical Examples
### Example 1: Checking Interface Status
To monitor real-time traffic on `ether1`:
```bash
/interface monitor-traffic ether1
# Output:
#  rx-packets-per-second: 150
#  tx-packets-per-second: 100
```

### Example 2: Exporting Configuration
To export and download the configuration:
```bash
/export compact file=config
scp admin@192.168.1.1:/config.rsc .
```

### Example 3: Checking Active DHCP Leases
To list active DHCP leases:
```bash
/ip dhcp-server lease print
# Output:
#  0 address=192.168.1.100 mac-address=00:11:22:33:44:55
```

## FAQ
### How do I enable SSH on MikroTik?
Run:
```bash
/ip service set ssh disabled=no
```

### How can I automate data collection?
Use scripts (as shown above) or the MikroTik API. Refer to [MikroTik API documentation](https://wiki.mikrotik.com/wiki/Manual:API).

### How do I secure exported configuration files?
Remove sensitive data (e.g., passwords, VPN keys) manually or use `/export hide-sensitive` to exclude them.

## Tips and Best Practices
- **Regular Backups**: Always create backups before changes:
  ```bash
  /system backup save name=backup
  ```
- **Verbose Output**: Use `print detail` or `/export verbose` for comprehensive data.
- **Log Analysis**: Regularly check `/log print` for troubleshooting.
- **Secure SSH**: Restrict SSH access to specific IPs:
  ```bash
  /ip firewall filter add chain=input action=accept protocol=tcp dst-port=22 src-address=<ALLOWED_IP>
  ```
- **Automation**: Schedule scripts using `/system scheduler` for periodic data collection.

## References
- [MikroTik Wiki](https://wiki.mikrotik.com)
- [MikroTik Forum](https://forum.mikrotik.com)
- Run `/help` in RouterOS CLI for command-specific details.

---

## Contributing
Contributions are welcome! Submit issues or pull requests to enhance this guide.

## License
This project is licensed under the MIT License.