# Configuration SW-LAB

## Infos

- Modèle : Cisco WS-C2960-8TC-L
- IOS : 12.2(50) LAN Base (image k9 à vérifier avec `show version`)
- Hostname : SW-LAB

## État actuel

| Port | Description | VLAN | Mode |
|---|---|---|---|
| Fa0/1 | PC-PRINCIPAL | 10 | Access |
| Fa0/2 | PROXMOX | 20 | Access |
| Fa0/3 | DMZ | 30 | Access |
| Fa0/4 | ADMIN | 40 | Access |
| Fa0/5, Fa0/6 | — | 1 | Access |
| Fa0/7 | BOX-INTERNET | 1 | Access |
| Fa0/8 | TRUNK-OPNSENSE | 10,20,30,40 | Trunk |

- IP de management : 192.168.1.2/24, dans le VLAN 1 (le réseau de la box)
- Accès distant : Telnet

### Écarts avec la cible

- Le management est dans le VLAN 1, au même endroit que la box, et en Telnet : il doit passer dans le VLAN 40.
- Proxmox est en access VLAN 20 : il doit recevoir le trunk.
- Le trunk est sur un port 100 Mb/s (Fa0/8) : il doit passer sur Gi0/1.
- Le VLAN natif du trunk est le VLAN 1 : il doit passer au VLAN 999.
- Les ports inutilisés sont actifs : ils doivent être désactivés.
- Les VLANs 50, 60 et 999 ne sont pas encore créés.

## Configuration cible

| Port | Description | VLAN | Mode |
|---|---|---|---|
| Gi0/1 | TRUNK-PROXMOX | 10,20,30,40,50,60 (natif 999) | Trunk |
| Fa0/1 | PC-PRINCIPAL | 10 | Access |
| Fa0/2 | PBS | 40 | Access |
| Fa0/3 → Fa0/8 | UNUSED | 999 | Access, shutdown |

Option B (serveur à un seul port réseau) : un port pour la box en access VLAN 99, et VLAN 99 ajouté au trunk.

- Management : SVI `Vlan40` en 192.168.40.2/24, `ip default-gateway 192.168.40.1`, `interface Vlan1` en shutdown
- Accès distant : SSH si l'image est k9. Sinon, Telnet restreint au PC principal avec `access-class` sur les lignes VTY, en attendant
- Le `running-config` nettoyé sera ajouté dans ce dossier une fois la configuration appliquée
