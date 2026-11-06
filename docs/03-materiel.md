# Inventaire matériel

| Équipement | Modèle | Statut | Rôle |
|---|---|---|---|
| Switch | Cisco WS-C2960-8TC-L | VLANs créés, sécurisation à faire | Commutation, VLANs |
| Serveur principal | À définir | En suspens | Proxmox + OPNsense, toujours allumé |
| Ancien PC | PC 4 Go RAM | Disponible | Proxmox Backup Server |
| Routeur | On Networks | Récupéré | Tests réseau, hors architecture principale |
| Câble console | USB → RJ45 Cisco | Reçu | Configuration du switch en CLI |

## Serveur principal — critères de choix

- **2 ports réseau** de préférence (voir l'option A dans l'[architecture](01-architecture.md)), avec des puces Intel bien supportées par OPNsense/FreeBSD
- **32 Go de RAM** (16 Go est trop juste avec OPNsense, Wazuh et les services du quotidien)
- SSD NVMe pour les VMs, avec un emplacement libre pour ajouter un disque de données plus tard
- Faible consommation au repos (de l'ordre de 10 à 15 W), puisqu'il reste allumé en permanence

## Ancien PC (PBS)

- Prévoir un **disque dédié aux sauvegardes**, dont la capacité dépend des données de Nextcloud et Immich
- Il peut être réveillé par Wake-on-LAN depuis OPNsense pour la fenêtre de sauvegarde, puis éteint (voir [sauvegardes](07-sauvegardes.md))

## Switch — notes

- IOS 12.2(50) LAN Base
- Pour SSH, il faut une image **crypto (k9)**, et pas seulement une version plus récente : à vérifier avec `show version`
- Ports Fa0/1 à Fa0/8 en 100 Mb/s, un seul port gigabit (Gi0/1), qui est réservé au trunk vers Proxmox

## Matériel retiré du projet

- **Raspberry Pi Zero 2 W** : 512 Mo de RAM et du WiFi uniquement, pas adapté pour le DNS ou le VPN. Ses rôles sont repris par OPNsense (WireGuard, Wake-on-LAN) et un LXC (Pi-hole).
