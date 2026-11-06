# Plan d'adressage IP

## Convention

Dans chaque VLAN :

| Plage | Usage |
|---|---|
| `.1` | Passerelle (interface OPNsense) |
| `.2` → `.99` | IP fixes (serveurs, équipements) |
| `.100` → `.199` | DHCP |
| `.200` → `.254` | Réservé |

## VLANs

| VLAN | Nom | Réseau | Passerelle | DHCP |
|---|---|---|---|---|
| 10 | LAN | 192.168.10.0/24 | 192.168.10.1 | Oui |
| 20 | SERVICES | 192.168.20.0/24 | 192.168.20.1 | Non (IP fixes) |
| 30 | DMZ | 192.168.30.0/24 | 192.168.30.1 | Non |
| 40 | ADMIN | 192.168.40.0/24 | 192.168.40.1 | Non (IP fixes) |
| 50 | MONITORING | 192.168.50.0/24 | 192.168.50.1 | Non (IP fixes) |
| 60 | LAB-ATTAQUE | 192.168.60.0/24 | 192.168.60.1 | Oui |
| — | VPN WireGuard | 192.168.100.0/24 | 192.168.100.1 | — |
| — | WAN (réseau de la box) | 192.168.1.0/24 | 192.168.1.1 (box) | Par la box |

## IP fixes prévues

| Machine | IP | VLAN |
|---|---|---|
| OPNsense (WAN) | Attribuée par la box (réservation DHCP sur la box) | WAN |
| OPNsense (une interface par VLAN) | 192.168.X.1 | Tous |
| Switch SW-LAB (management) | 192.168.40.2 | 40 |
| Proxmox | 192.168.40.10 | 40 |
| Proxmox Backup Server (ancien PC) | 192.168.40.20 | 40 |
| PC principal | 192.168.10.10 (réservation DHCP) | 10 |
| Pi-hole | 192.168.20.53 | 20 |
| Vaultwarden | 192.168.20.10 | 20 |
| Paperless-NGX | 192.168.20.11 | 20 |
| Nextcloud | 192.168.20.12 | 20 |
| Immich | 192.168.20.13 | 20 |
| Gitea | 192.168.20.14 | 20 |
| Homepage | 192.168.20.15 | 20 |
| Prometheus | 192.168.50.10 | 50 |
| Grafana | 192.168.50.11 | 50 |
| Wazuh | 192.168.50.20 | 50 |
| Kali | 192.168.60.10 | 60 |

## Notes

- Le PC principal a une réservation DHCP fixe : c'est la seule machine du LAN autorisée à joindre le VLAN ADMIN (voir la [matrice des flux](06-flux.md)).
- DNS : tous les VLANs utilisent Pi-hole (192.168.20.53), qui s'appuie sur Unbound (OPNsense). Exception : le VLAN LAB-ATTAQUE utilise directement Unbound sur sa passerelle, pour ne pas avoir à joindre le VLAN SERVICES.
