# Lab 11.9.3 – VLSM Design and Implementation Practice

## Objective
Design an addressing scheme with **VLSM** (Variable Length Subnet Masking) from one block, then configure the routers, switches and hosts.

## Given
- Network: **10.11.48.0/24**
- Topology (one of three random versions): Building1 and Building2 routers joined by a serial link, each with two LANs.
- Host requirements seen in my topology: **60, 30, 14 and 6 hosts**.

## VLSM method
1. Sort the subnets from largest to smallest.
2. Pick the smallest mask that fits each (2^h − 2 ≥ hosts).
3. Assign them in order, each starting right after the previous one.
4. Use a /30 for the point-to-point router link.

## Address plan
| Subnet | Hosts needed | Mask | Network | Usable range | Broadcast |
|--------|--------------|------|---------|--------------|-----------|
| 60 hosts (Host-D side) | 60 | /26 (255.255.255.192) | 10.11.48.0 | .1 – .62 | .63 |
| 30 hosts (Host-B side) | 30 | /27 (255.255.255.224) | 10.11.48.64 | .65 – .94 | .95 |
| 14 hosts (Host-A side) | 14 | /28 (255.255.255.240) | 10.11.48.96 | .97 – .110 | .111 |
| 6 hosts (Host-C side) | 6 | /29 (255.255.255.248) | 10.11.48.112 | .113 – .118 | .119 |
| Serial link | 2 | /30 (255.255.255.252) | 10.11.48.120 | .121 – .122 | .123 |

> The exact assignment depends on which of the three topologies you received; follow the same method with your host counts.

## Commands
Router LAN interfaces (example for the 60-host LAN):
```
Building2(config)# interface g0/0
Building2(config-if)# ip address 10.11.48.1 255.255.255.192
Building2(config-if)# no shutdown
```
Serial link:
```
Building1(config)# interface s0/0/0
Building1(config-if)# ip address 10.11.48.121 255.255.255.252
Building1(config-if)# no shutdown
```
Switch management and gateway:
```
ASW-1(config)# interface vlan 1
ASW-1(config-if)# ip address <address in its subnet> <mask>
ASW-1(config-if)# no shutdown
ASW-1(config)# ip default-gateway <router address>
```
Hosts: set IP, mask and default gateway in Desktop → IP Configuration.

Verification: `show ip interface brief`, `show ip route`, `ping` between hosts.

## Result
Addresses fit each LAN with minimal waste. Completion: 100%.
