# Lab 10.3.4 – Connect a Router to a LAN

## Objective
- Part 1: Display router information
- Part 2: Configure router interfaces
- Part 3: Verify the configuration

## Addressing table
| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| R1 | G0/0 | 192.168.10.1 | 255.255.255.0 |
| R1 | G0/1 | 192.168.11.1 | 255.255.255.0 |
| R1 | S0/0/0 (DCE) | 209.165.200.225 | 255.255.255.252 |
| R2 | G0/0 | 10.1.1.1 | 255.255.255.0 |
| R2 | G0/1 | 10.1.2.1 | 255.255.255.0 |
| R2 | S0/0/0 | 209.165.200.226 | 255.255.255.252 |
| PC1 | NIC | 192.168.10.10 | gateway 192.168.10.1 |
| PC2 | NIC | 192.168.11.10 | gateway 192.168.11.1 |
| PC3 | NIC | 10.1.1.10 | gateway 10.1.1.1 |
| PC4 | NIC | 10.1.2.10 | gateway 10.1.2.1 |

## Topology
Two routers (R1, R2) joined by a serial link. Each router has two LANs, each with a switch and a PC.

## Steps and commands

### Part 1 – Display information
```
R1# show ip interface brief
R1# show interfaces
R1# show ip route
R1# show running-config
```
- Shows which interfaces are up, their IPs, and the routing table.

### Part 2 – Configure interfaces
```
R1# configure terminal
R1(config)# interface g0/0
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# description Link to LAN 192.168.10.0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface g0/1
R1(config-if)# ip address 192.168.11.1 255.255.255.0
R1(config-if)# description Link to LAN 192.168.11.0
R1(config-if)# no shutdown
```
- `ip address` – assigns the LAN gateway address.
- `description` – a label for documentation.
- `no shutdown` – routers' interfaces are off by default, so this is required.

### Part 3 – Verify
```
R1# show ip interface brief
R1# show ip route
R1# ping 192.168.10.10
PC1> ping 192.168.11.10
```
- Connected networks appear in the routing table with code `C` (and `L` for local).
- Pings between LANs on the same router succeed.

## Result
Both LAN interfaces are up/up and each router routes between its own LANs. Completion: 100%.
