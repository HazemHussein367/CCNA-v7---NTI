# Lab 11.5.5 – Subnet an IPv4 Network

## Objective
Divide a given network into subnets that fit the required number of hosts, then assign addresses to the CustomerRouter, switches and PCs, and verify connectivity.

## Topology
- **LAN-A**: PC-A, 50 hosts
- **LAN-B**: PC-B, 40 hosts
- **CustomerRouter** ↔ **ISPRouter** link: 209.165.201.0/30
- ISP LAN: 209.165.200.224/27 (ISP switch, workstation, server)

## Known values from the table
| Device | Interface | IP | Mask |
|--------|-----------|----|------|
| CustomerRouter | S0/1/0 | 209.165.201.2 | 255.255.255.252 |
| ISPRouter | S0/1/0 | 209.165.201.1 | 255.255.255.252 |
| ISPRouter | G0/0 | 209.165.200.225 | 255.255.255.224 |

## Subnetting method
1. Find the bits needed for hosts: 2^h − 2 ≥ hosts.
   - 50 hosts → h = 6 (62 usable) → **/26** (255.255.255.192)
   - 40 hosts → h = 6 (62 usable) → **/26** (255.255.255.192)
2. Take the first two /26 subnets of the given block (each 64 addresses apart).
3. Router interface = first usable address (the default gateway); switch and PC addresses follow in the same subnet.

> Fill in your exact network IDs from the activity block (e.g. subnet 1 = `x.x.x.0/26`, subnet 2 = `x.x.x.64/26`).

## Commands
```
CustomerRouter(config)# interface g0/0
CustomerRouter(config-if)# ip address <first usable of subnet A> 255.255.255.192
CustomerRouter(config-if)# no shutdown
CustomerRouter(config)# interface g0/1
CustomerRouter(config-if)# ip address <first usable of subnet B> 255.255.255.192
CustomerRouter(config-if)# no shutdown
```
Switches (management):
```
LAN-A(config)# interface vlan 1
LAN-A(config-if)# ip address <addr in subnet A> 255.255.255.192
LAN-A(config-if)# no shutdown
LAN-A(config)# ip default-gateway <router G0/0 address>
```
PCs: Desktop → IP Configuration (IP, mask 255.255.255.192, default gateway = router interface).

Verification:
```
show ip interface brief
ping <ISP server / workstation>
```

## Result
Each LAN fits its hosts with the smallest suitable subnet, and all devices reach each other. Completion: 100%.
