# Configuration SW-LAB

## Infos
- Modèle : Cisco WS-C2960-8TC-L
- IOS : 12.2(50) LAN Base
- Hostname : SW-LAB
- IP management : 192.168.1.2/24

## VLANs configurés

| VLAN | Nom | Port assigné |
|---|---|---|
| 10 | LAN | Fa0/1 |
| 20 | LAB | Fa0/2 |
| 30 | DMZ | Fa0/3 |
| 40 | ADMIN | Fa0/4 |
| 1 | DEFAULT | Fa0/5, Fa0/6, Fa0/7, Fa0/8 |

## Ports

| Port | Description | VLAN | Mode |
|---|---|---|---|
| Fa0/1 | PC-PRINCIPAL | 10 | Access |
| Fa0/2 | PROXMOX | 20 | Access |
| Fa0/3 | DMZ | 30 | Access |
| Fa0/4 | ADMIN | 40 | Access |
| Fa0/7 | BOX-INTERNET | 1 | Access |
| Fa0/8 | TRUNK-OPNSENSE | 10,20,30,40 | Trunk |

## Notes
- SSH non supporté sur IOS 12.2(50)
- Telnet activé temporairement
- Mise à jour IOS vers 15.x prévue via TFTP avec Proxmox