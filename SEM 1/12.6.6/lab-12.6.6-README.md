# Lab 12.6.6 – Configure IPv6 Addressing

## Objective
- Part 1: Configure IPv6 addressing on the router
- Part 2: Configure IPv6 addressing on servers
- Part 3: Configure IPv6 addressing on clients
- Part 4: Test and verify network connectivity

## Addressing table
| Device | Interface | IPv6 Address/Prefix | Default Gateway |
|--------|-----------|---------------------|-----------------|
| R1 | G0/0 | 2001:db8:1:1::1/64 (link-local fe80::1) | N/A |
| R1 | G0/1 | 2001:db8:1:2::1/64 (link-local fe80::1) | N/A |
| R1 | S0/0/0 | 2001:db8:1:a001::2/64 (link-local fe80::1) | N/A |
| Sales | NIC | 2001:db8:1:1::2/64 | fe80::1 |
| Billing | NIC | 2001:db8:1:1::3/64 | fe80::1 |
| Accounting | NIC | 2001:db8:1:1::4/64 | fe80::1 |
| Design | NIC | 2001:db8:1:2::2/64 | fe80::1 |
| Engineering | NIC | 2001:db8:1:2::3/64 | fe80::1 |
| CAD | NIC | 2001:db8:1:2::4/64 | fe80::1 |
| ISP | S0/0/0 | 2001:db8:1:a001::1 | fe80::1 |

## Steps and commands

### Part 1 – Router
```
R1# configure terminal
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:1:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface g0/1
R1(config-if)# ipv6 address 2001:db8:1:2::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface s0/0/0
R1(config-if)# ipv6 address 2001:db8:1:a001::2/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
```
- `ipv6 unicast-routing` – lets the router forward IPv6 packets.
- `ipv6 address ... /64` – global unicast address.
- `ipv6 address fe80::1 link-local` – a simple, memorable link-local address (also used as the hosts' gateway).

### Parts 2 and 3 – Servers and clients
On each device: Desktop → IP Configuration → IPv6 Address (with /64) and IPv6 Gateway `fe80::1`.

### Part 4 – Verify
```
R1# show ipv6 interface brief
R1# ping 2001:db8:1:a001::1
PC> ipconfig
PC> ping 2001:db8:1:2::2
```
- `show ipv6 interface brief` – status and IPv6 addresses.
- Pings confirm hosts reach each other and the ISP.

## Result
All devices are addressed and can communicate over IPv6. Completion: 100%.
