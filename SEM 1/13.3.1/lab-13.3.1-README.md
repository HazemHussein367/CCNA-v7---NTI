# Lab 13.3.1 – Use ICMP to Test and Correct Network Connectivity

## Objective
Use **ICMP** tools (`ping`, `tracert`) to find and fix connectivity problems in a network with IPv4 and IPv6.

## Topology
Three routers: RTR-1 (connects to the Corporate server through the cloud), RTR-2 and RTR-3, each with two LANs (IPv4 and IPv6 networks).

## Partial addressing (from the screenshot)
| Device | Interface | IPv4 | IPv6 |
|--------|-----------|------|------|
| RTR-1 | G0/0/0 | 192.168.1.1/24 | 2001:db8:4::1/64 |
| RTR-1 | S0/1/0 | 10.10.2.2/30 | 2001:db8:2::2/126 |
| RTR-1 | S0/1/1 | 10.10.3.1/30 | 2001:db8:3::1/126 |
| RTR-2 | G0/0/0 | 10.10.1.1/24 | – |
| RTR-2 | G0/0/1 | – | 2001:db8:1::1/64 |
| RTR-2 | S0/1/0 | 10.10.2.1/30 | 2001:db8:2::1/126 |
| Corporate Server | – | 203.0.113.100 | 2001:db8:acad::100 |

## ICMP tools used
| Command | Where | Purpose |
|---------|-------|---------|
| `ipconfig` | PC | Check IP, mask, gateway |
| `ping <ip>` | PC / router | Test end-to-end reachability (ICMP echo) |
| `tracert <ip>` | PC | Show each hop to the destination |
| `traceroute <ip>` | Router | Same, from the router |
| `show ip interface brief` | Router | Interface status/IPv4 |
| `show ipv6 interface brief` | Router | Interface status/IPv6 |
| `show ip route` / `show ipv6 route` | Router | Verify routes |
| `show running-config` | Router | Find wrong addresses/routes |

## Troubleshooting workflow
1. `ping` the local gateway → if it fails, check the PC's IP/gateway and the cable.
2. `ping` the next router → if it fails, check interface status and addresses on both ends.
3. `tracert` to the server → the last responding hop shows where the problem begins.
4. On that router check `show ip route`, `show ip interface brief`, then fix the fault, for example:
```
Router(config)# interface s0/1/0
Router(config-if)# ip address 10.10.2.2 255.255.255.252
Router(config-if)# no shutdown
Router(config)# ip route 0.0.0.0 0.0.0.0 <next-hop>
Router(config)# ipv6 route ::/0 <next-hop>
```
5. Re-test with `ping` and `tracert` for both IPv4 and IPv6.

## Result
Connectivity restored from all hosts to the Corporate Server over IPv4 and IPv6. Completion: 100%.
