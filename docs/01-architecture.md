# Architecture du lab

## Topologie
'''
Internet
|
[Box Internet] — Fa0/7
|
[Switch Cisco SW-LAB]
|
[PC Proxmox] — Fa0/2 (VLAN 20)
|
[OPNsense VM]
|
├── VLAN 10 — LAN 192.168.10.0/24
├── VLAN 20 — LAB 192.168.20.0/24
├── VLAN 30 — DMZ 192.168.30.0/24
└── VLAN 40 — ADMIN 192.168.40.0/24
'''

## Rôles
- **Box** : accès internet uniquement
- **Switch Cisco** : commutation, segmentation VLAN
- **OPNsense** : firewall, routage inter-VLAN, VPN, DHCP, DNS
- **Proxmox** : hyperviseur principal
- **Raspberry Pi** (prévu) : WireGuard, Pi-hole, WoL