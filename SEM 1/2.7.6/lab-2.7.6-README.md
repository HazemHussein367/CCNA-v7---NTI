# Lab 2.7.6 – Implement Basic Connectivity

## Objective
- Part 1: Perform a basic configuration on S1 and S2
- Part 2: Configure the PCs
- Part 3: Configure the switch management interface

## Addressing table
| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| S1 | VLAN 1 | 192.168.1.253 | 255.255.255.0 |
| S2 | VLAN 1 | 192.168.1.254 | 255.255.255.0 |
| PC1 | NIC | 192.168.1.1 | 255.255.255.0 |
| PC2 | NIC | 192.168.1.2 | 255.255.255.0 |

## Topology
S1 ↔ S2 (switch to switch), PC1 on S1, PC2 on S2.

## Steps and commands

### Part 1 – Basic switch config (S1 and S2)
```
Switch> enable
Switch# configure terminal
Switch(config)# hostname S1
S1(config)# enable secret class
S1(config)# line console 0
S1(config-line)# password cisco
S1(config-line)# login
S1(config-line)# exit
S1(config)# line vty 0 15
S1(config-line)# password cisco
S1(config-line)# login
S1(config-line)# exit
S1(config)# service password-encryption
S1(config)# banner motd #Authorized access only#
```
Repeat with `hostname S2` on S2.

### Part 2 – Configure the PCs
On each PC: **Desktop → IP Configuration** and set the IP and mask from the table (PC1 = 192.168.1.1, PC2 = 192.168.1.2, mask 255.255.255.0).

### Part 3 – Switch management interface (SVI)
```
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.1.253 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# end
S1# copy running-config startup-config
```
(S2 uses 192.168.1.254.)
- `interface vlan 1` – the virtual interface used to manage the switch over the network.
- `ip address` – gives the switch an IP.
- `no shutdown` – activates the interface.

### Verification
```
S1# show ip interface brief
S1# ping 192.168.1.254
PC> ping 192.168.1.2
```
- `show ip interface brief` – confirms VLAN 1 is up/up with the right address.
- `ping` – confirms connectivity between devices.

## Result
All four devices can ping each other. Completion: 100%.
