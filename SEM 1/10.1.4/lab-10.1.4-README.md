# Lab 10.1.4 – Configure Initial Router Settings

## Objective
- Part 1: Verify the default router configuration
- Part 2: Configure and verify the initial router configuration
- Part 3: Save the running configuration file

## Topology
PCA connected to R1 with a **console cable** (RS-232 on PCA → Console on R1).

## Steps and commands

### Part 1 – Connect and verify defaults
1. Choose a Console cable, click PCA → RS 232, click R1 → Console.
2. PCA → Desktop → Terminal → OK → press Enter.
```
Router> enable
Router# show running-config
Router# show startup-config
Router# show version
```
- Shows the default (empty) configuration and device details.

### Part 2 – Initial configuration
```
Router# configure terminal
Router(config)# hostname R1
R1(config)# security passwords min-length 10
R1(config)# enable secret <password>
R1(config)# line console 0
R1(config-line)# password <password>
R1(config-line)# login
R1(config-line)# exit
R1(config)# line vty 0 4
R1(config-line)# password <password>
R1(config-line)# login
R1(config-line)# exit
R1(config)# service password-encryption
R1(config)# banner motd #Unauthorized access is prohibited#
```
- `security passwords min-length 10` – forces passwords of at least 10 characters.
- `enable secret` – encrypted privileged-mode password.
- `line console 0` / `line vty` – protect local and remote access.
- `service password-encryption` – hides plain-text passwords in the config.
- `banner motd` – warns users that access is prohibited.

### Part 3 – Save
```
R1# copy running-config startup-config
R1# show startup-config
```
Saves to NVRAM so the configuration survives a reboot.

## Result
R1 has a hostname, protected console/VTY/privileged access, a banner, and a saved configuration. Completion: 100%.
