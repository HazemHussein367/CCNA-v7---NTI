# Lab 2.5.5 – Configure Initial Switch Settings

## Objective
- Part 1: Verify the default switch configuration
- Part 2: Configure a basic switch configuration
- Part 3: Configure a MOTD banner
- Part 4: Save configuration files to NVRAM
- Part 5: Configure S2

## Topology
Two switches (S1, S2) connected together, each with a PC (PC1, PC2).

## What this lab teaches
How to secure and name a new switch, and how to save the configuration.

## Steps and commands

### Part 1 – Verify the default config
```
Switch> enable
Switch# show running-config
Switch# show startup-config
Switch# show version
Switch# show interfaces
Switch# show vlan brief
```
- `enable` – enter privileged EXEC mode.
- `show running-config` – display the current configuration in RAM.
- `show startup-config` – display the saved configuration in NVRAM (empty by default).
- `show version` – IOS version, uptime, hardware info.
- `show interfaces` – status and counters of every port.
- `show vlan brief` – VLANs and which ports belong to them.

### Part 2 – Basic configuration
```
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
```
- `hostname S1` – names the switch.
- `enable secret class` – password for privileged mode (stored encrypted).
- `line console 0` / `password` / `login` – require a password on the console port.
- `line vty 0 15` – the 16 virtual terminal lines used for remote access.
- `service password-encryption` – encrypts the plain-text line passwords.

### Part 3 – MOTD banner
```
S1(config)# banner motd #Authorized access only!#
```
Shows a legal warning before anyone logs in.

### Part 4 – Save the config
```
S1# copy running-config startup-config
```
Without this the configuration is lost on reload.

Optional (to test):
```
S1# erase startup-config
S1# reload
```

### Part 5 – Configure S2
Repeat the same steps on S2 (`hostname S2`, passwords, banner, save).

## Result
Both switches are named, password-protected, show a banner, and the config is saved. Packet Tracer completion: 100%.
