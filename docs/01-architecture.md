# Architecture du lab

## Principes

- **Segmentation par VLAN** : chaque usage a son réseau, et tout passage d'un VLAN à un autre traverse le firewall.
- **Refus par défaut** : un flux n'est ouvert que s'il figure dans la [matrice des flux](06-flux.md).
- **Firewall toujours allumé** : OPNsense tourne sur le serveur principal, qui reste allumé en permanence. Sans lui, aucun VLAN n'a accès à Internet.
- **Rien d'exposé sur Internet au départ** : l'accès à distance passe uniquement par le VPN WireGuard sur OPNsense.
- **Les sauvegardes sur une autre machine** : l'ancien PC sert de serveur de sauvegarde (Proxmox Backup Server).

## Topologie cible

Le serveur principal n'est pas encore choisi. Le raccordement au WAN dépend de son nombre de ports réseau :

### Option A — serveur à 2 ports réseau (préférée)

```
Internet
   |
[Box Internet]
   |  (câble direct, hors switch)
[NIC 1 = WAN]
[Serveur Proxmox] ── OPNsense VM (firewall, routage inter-VLAN, VPN, DHCP)
[NIC 2 = trunk]
   |  VLANs 10,20,30,40,50,60 taggés — VLAN natif 999
[Switch Cisco SW-LAB — Gi0/1]
   |
   ├── Fa0/1 — PC principal          (VLAN 10 LAN)
   ├── Fa0/2 — Ancien PC / PBS       (VLAN 40 ADMIN)
   └── Fa0/3 → Fa0/8 — désactivés    (VLAN 999 PARKING)
```

### Option B — serveur à 1 seul port réseau

Même chose, mais la box se branche sur le switch, sur un port en access dans un **VLAN 99 WAN** dédié. Ce VLAN n'est présent que sur ce port et sur le trunk vers Proxmox. Seule l'interface WAN d'OPNsense y a accès.

## VLANs

| VLAN | Nom | Rôle |
|---|---|---|
| 10 | LAN | Postes personnels, téléphones |
| 20 | SERVICES | Services auto-hébergés (Vaultwarden, Paperless, Nextcloud, Immich, Gitea, Pi-hole…) |
| 30 | DMZ | Vide pour l'instant. Plus tard : reverse proxy exposé sur Internet |
| 40 | ADMIN | Interfaces de gestion uniquement : Proxmox, switch, OPNsense, PBS |
| 50 | MONITORING | Prometheus, Grafana, Wazuh |
| 60 | LAB-ATTAQUE | Kali et VMs vulnérables, isolé du reste |
| 99 | WAN | Uniquement en option B |
| 999 | PARKING | VLAN natif des trunks et ports inutilisés — aucun équipement |
| 1 | — | Non utilisé |

Le détail des adresses se trouve dans le [plan d'adressage](02-adressage-ip.md).

## Rôles des équipements

| Équipement | Rôle |
|---|---|
| Box Internet | Accès Internet uniquement, rien d'autre branché dessus |
| Switch Cisco SW-LAB | Commutation, VLANs, trunk vers Proxmox (port gigabit Gi0/1) |
| Serveur Proxmox | Hyperviseur principal, toujours allumé |
| OPNsense (VM) | Firewall, routage inter-VLAN, DHCP, DNS récursif (Unbound), VPN WireGuard |
| Ancien PC 4 Go | Proxmox Backup Server |

## Choix et justifications

- **Trunk sur Gi0/1** : c'est le seul port gigabit du switch. Les ports Fa0/x sont limités à 100 Mb/s, ce qui briderait tout le trafic inter-VLAN qui fait l'aller-retour sur le trunk.
- **WireGuard sur OPNsense plutôt que sur une machine à part** : le trafic VPN arrive derrière le firewall et suit les mêmes règles que le reste.
- **Pi-hole en LXC, Unbound sur OPNsense en secours** : le DNS ne dépend pas d'un seul petit équipement.
- **Pas de Raspberry Pi Zero 2 W** : 512 Mo de RAM et pas de port Ethernet, il n'est pas adapté aux services critiques (DNS, VPN).
- **Prometheus et Grafana en LXC sur Proxmox** : l'ancien PC est réservé aux sauvegardes.
