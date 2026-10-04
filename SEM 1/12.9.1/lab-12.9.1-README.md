# Lab 12.9.1 – Implement a Subnetted IPv6 Addressing Scheme

## Objective
Take one IPv6 block, divide it into subnets, and configure routers and PCs so every device communicates.

## Given
- Block: **2001:db8:acad:00c8::/64** (R1 G0/0 is already `2001:db8:acad:00c8::1/64`)
- Link-local: R1 = `fe80::1`, R2 = `fe80::2` (on every interface)
- PC1–PC4 use **Auto Config** (SLAAC)

## Topology
Four LANs, each with a PC and a switch. PC1 and PC2 attach to R1; PC3 and PC4 attach to R2. R1 and R2 are joined by a serial link.

## Subnetting method
IPv6 subnetting uses the **Subnet ID** field (4th hextet). Increment it by 1 for each new subnet, keeping /64:
| Network | Subnet ID |
|---------|-----------|
| R1 G0/0 LAN | 00c8 |
| R1 G0/1 LAN | 00c9 |
| R2 LAN 1 | 00ca |
| R2 LAN 2 | 00cb |
| R1–R2 serial link | 00cc |

> Check these IDs against your table; the principle is "one subnet ID per link".

## Commands
```
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:c8::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config)# interface g0/1
R1(config-if)# ipv6 address 2001:db8:acad:c9::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config)# interface s0/0/0
R1(config-if)# ipv6 address 2001:db8:acad:cc::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
```
Repeat on R2 with its subnets and `fe80::2`.

PCs: Desktop → IP Configuration → **Auto Config** (they build their address from the router's advertised prefix).

Verification:
```
show ipv6 interface brief
show ipv6 route
ping <other PC's IPv6 address>
```

## Result
Every LAN has its own /64 and all PCs reach each other. Completion: 100%.
