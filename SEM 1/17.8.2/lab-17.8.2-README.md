# Lab 17.8.2 – Skills Integration Challenge

## Objective
Combine everything from the course: design IPv4 subnets, configure IPv6, secure devices, and verify connectivity to a web server.

## Topology
- Three LANs on **R1**: Staff (100 hosts, S1), Sales (50 hosts, S2), IT (25 hosts, S3)
- **R1** ↔ **Central** router (serial) ↔ Cloud ↔ Web server (www.cisco.pka, www.cisco6.pka)

## Partial addressing (from the screenshot)
| Device | Interface | IPv4 | IPv6 |
|--------|-----------|------|------|
| R1 | G0/0 | (your subnet) | 2001:db8:acad::1/64, fe80::1 |
| R1 | G0/1 | (your subnet) | 2001:db8:acad:1::1/64, fe80::1 |
| R1 | G0/2 | (your subnet) | 2001:db8:acad:2::1/64, fe80::1 |
| R1 | S0/0/1 | 172.16.1.2/30 | 2001:db8:2::1/64, fe80::1 |
| Central | S0/0/0 | 209.165.200.226/30 | 2001:db8:1::1/64 |
| Central | S0/0/1 | 172.16.1.1/30 | 2001:db8:2::2/64, fe80::2 |

## IPv4 subnet sizing
| LAN | Hosts | Bits | Mask |
|-----|-------|------|------|
| Staff | 100 | 7 | /25 (255.255.255.128) |
| Sales | 50 | 6 | /26 (255.255.255.192) |
| IT | 25 | 5 | /27 (255.255.255.224) |

Assign largest first (VLSM) from the block given in the activity.

## Commands
Hostname and security:
```
R1(config)# hostname R1
R1(config)# enable secret <password>
R1(config)# line console 0
R1(config-line)# password <password>
R1(config-line)# login
R1(config)# service password-encryption
R1(config)# banner motd #Authorized access only#
```
Interfaces (IPv4 + IPv6):
```
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0
R1(config-if)# ip address <Staff gateway> 255.255.255.128
R1(config-if)# ipv6 address 2001:db8:acad::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config)# interface s0/0/1
R1(config-if)# ip address 172.16.1.2 255.255.255.252
R1(config-if)# ipv6 address 2001:db8:2::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
```
Default routes towards Central:
```
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.1.1
R1(config)# ipv6 route ::/0 2001:db8:2::2
```
Switch management and gateway:
```
S1(config)# interface vlan 1
S1(config-if)# ip address <address> <mask>
S1(config-if)# no shutdown
S1(config)# ip default-gateway <R1 gateway>
```
Hosts: set IP, mask, gateway (IPv4) and IPv6 address/gateway `fe80::1`.

## Verification
```
show ip interface brief
show ipv6 interface brief
show ip route
show ipv6 route
ping www.cisco.pka
ping www.cisco6.pka
```
Open the web server in the PC browser to confirm both IPv4 and IPv6 work.

## Result
All three LANs reach the web server over IPv4 and IPv6. Completion: 100%.
