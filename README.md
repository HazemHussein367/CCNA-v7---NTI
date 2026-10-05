# CCNA v7 – Introduction to Networks (ITN)
## Packet Tracer Labs Write-Up

- **Student:** Hazem Hussein
- **Institute:** NTI
- **Tool:** Cisco Packet Tracer

This repository documents the Packet Tracer labs I completed during CCNA v7 *Introduction to Networks*. Every lab has its own README explaining the goal, the topology, the steps, and every command used.

## Lab index

| Lab | Title | Topic | README |
|-----|-------|-------|--------|
| 2.5.5 | Configure Initial Switch Settings | Basic switch config, passwords, MOTD | `lab-2.5.5-README.md` |
| 2.7.6 | Implement Basic Connectivity | Switch SVI, PC IPs, ping | `lab-2.7.6-README.md` |
| 10.1.4 | Configure Initial Router Settings | Basic router config, securing access | `lab-10.1.4-README.md` |
| 10.3.4 | Connect a Router to a LAN | Router interfaces, verification | `lab-10.3.4-README.md` |
| 11.5.5 | Subnet an IPv4 Network | IPv4 subnetting | `lab-11.5.5-README.md` |
| 11.9.3 | VLSM Design and Implementation Practice | VLSM addressing | `lab-11.9.3-README.md` |
| 12.6.6 | Configure IPv6 Addressing | IPv6 on router, servers, clients | `lab-12.6.6-README.md` |
| 12.9.1 | Implement a Subnetted IPv6 Addressing Scheme | IPv6 subnetting | `lab-12.9.1-README.md` |
| 13.3.1 | Use ICMP to Test and Correct Network Connectivity | ping, tracert, troubleshooting | `lab-13.3.1-README.md` |
| 16.5.1 | Secure Network Devices | Passwords, SSH, login protection | `lab-16.5.1-README.md` |
| 17.8.2 | Skills Integration Challenge | Everything combined | `lab-17.8.2-README.md` |

## IOS CLI modes (used in every lab)

| Mode | Prompt | How to enter | Purpose |
|------|--------|--------------|---------|
| User EXEC | `Switch>` | default | Limited viewing |
| Privileged EXEC | `Switch#` | `enable` | All show commands, save config |
| Global config | `Switch(config)#` | `configure terminal` | Device-wide settings |
| Line config | `(config-line)#` | `line console 0` / `line vty 0 15` | Console / remote access |
| Interface config | `(config-if)#` | `interface g0/0` | Per-interface settings |

Useful navigation: `exit` (go back one level), `end` or `Ctrl+Z` (back to Privileged EXEC), `?` (help), `Tab` (auto-complete).

## Most-used commands across all labs

| Command | What it does |
|---------|--------------|
| `enable` | Enter privileged EXEC mode |
| `configure terminal` | Enter global configuration mode |
| `hostname NAME` | Set the device name |
| `enable secret PASS` | Encrypted privileged-mode password |
| `line console 0` + `password` + `login` | Protect the console port |
| `line vty 0 15` + `password` + `login` | Protect remote (Telnet/SSH) access |
| `service password-encryption` | Encrypt plain-text passwords in the config |
| `banner motd #text#` | Warning message shown at login |
| `copy running-config startup-config` | Save the config to NVRAM |
| `show running-config` | Show current config (RAM) |
| `show startup-config` | Show saved config (NVRAM) |
| `show ip interface brief` | Interface status and IPv4 addresses |
| `show ipv6 interface brief` | Interface status and IPv6 addresses |
| `show ip route` / `show ipv6 route` | Routing table |
| `interface TYPE NUM` | Enter interface configuration |
| `ip address IP MASK` | Assign IPv4 address |
| `ipv6 address ADDR/PREFIX` | Assign IPv6 address |
| `no shutdown` | Turn an interface on |
| `ping DEST` | Test reachability with ICMP |
| `tracert DEST` (PC) / `traceroute DEST` (router) | Show the path hop by hop |
| `ipconfig` / `ipconfig /all` | Show a PC's IP settings |

## Skills practiced
- Basic device configuration and security
- IPv4 / IPv6 addressing, subnetting and VLSM
- Router and switch interface configuration
- SSH, login blocking and password protection
- Verification and troubleshooting with `show`, `ping` and `tracert`

> Note: the commands in each lab README follow the standard Cisco lab instructions and the details visible in the screenshots. If your own solution used different names or addresses, adjust them to match your saved `.pka` files.
