# Lab 16.5.1 – Secure Network Devices

## Objective
Secure the router (RTR-A) and switch (SW-1) with strong passwords, SSH, banners and login protection, then verify remote access from the Remote PC.

## Topology
RTR-A ↔ SW-2 ↔ Remote PC, and RTR-A ↔ SW-1 with a PC and a Laptop.

## Router requirements and commands
```
Router> enable
Router# configure terminal
Router(config)# hostname RTR-A
RTR-A(config)# no ip domain-lookup
RTR-A(config)# security passwords min-length 10
RTR-A(config)# enable secret @Cons1234!
RTR-A(config)# line console 0
RTR-A(config-line)# password @Cons1234!
RTR-A(config-line)# login
RTR-A(config-line)# exec-timeout 7 0
RTR-A(config-line)# exit
RTR-A(config)# banner motd #Unauthorized access is prohibited#
RTR-A(config)# service password-encryption
RTR-A(config)# username NETadmin secret LogAdmin19
RTR-A(config)# ip domain-name security.com
RTR-A(config)# crypto key generate rsa
   (modulus: 1024)
RTR-A(config)# line vty 0 15
RTR-A(config-line)# transport input ssh
RTR-A(config-line)# login local
RTR-A(config-line)# exec-timeout 7 0
RTR-A(config-line)# exit
RTR-A(config)# login block-for 45 attempts 3 within 100
```
| Command | Meaning |
|---------|---------|
| `no ip domain-lookup` | Stops IOS trying to resolve mistyped commands as hostnames |
| `security passwords min-length 10` | New passwords must be ≥ 10 chars |
| `enable secret` | Encrypted privileged-mode password |
| `exec-timeout 7 0` | Close idle sessions after exactly 7 minutes |
| `banner motd` | Legal warning about unauthorized access |
| `service password-encryption` | Encrypt all plain-text passwords |
| `username NETadmin secret LogAdmin19` | Local account with an encrypted password |
| `ip domain-name security.com` | Needed to generate RSA keys |
| `crypto key generate rsa` (1024) | Creates SSH keys |
| `transport input ssh` | Only SSH allowed on VTY lines (no Telnet) |
| `login local` | Authenticate with the local username/password |
| `login block-for 45 attempts 3 within 100` | Block logins 45 s after 3 failures within 100 s |

(Also enable SSH version 2 if requested: `ip ssh version 2`.)

## Switch requirements and commands
```
Switch(config)# hostname SW-1
SW-1(config)# interface range f0/2-24, g0/1-2
SW-1(config-if-range)# shutdown
SW-1(config-if-range)# exit
SW-1(config)# interface vlan 1
SW-1(config-if)# ip address <address from table> <mask>
SW-1(config-if)# no shutdown
SW-1(config-if)# exit
SW-1(config)# ip default-gateway <router address>
SW-1(config)# enable secret @Cons1234!
SW-1(config)# username NETadmin secret LogAdmin19
SW-1(config)# ip domain-name security.com
SW-1(config)# crypto key generate rsa
SW-1(config)# line vty 0 15
SW-1(config-line)# transport input ssh
SW-1(config-line)# login local
```
- `shutdown` on unused ports – prevents unauthorized devices from connecting.
- `interface vlan 1` + `ip default-gateway` – makes the switch manageable from other networks.
- Hosts on both LANs must be able to ping the switch management interface.

## Verification
```
Remote PC> ping <SW-1 address>
Remote PC> ssh -l NETadmin <SW-1 address>
show ip ssh
show running-config
```

## Result
Both devices accept only SSH with the NETadmin account, are protected against brute-force login, and unused ports are shut down.
