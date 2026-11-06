# Liste des VMs et LXC

Tous ces services sont hébergés sur le serveur Proxmox principal, sauf indication contraire.

| Service | Type | VLAN | IP | Rôle | Phase |
|---|---|---|---|---|---|
| OPNsense | VM | Tous (une interface par VLAN) | 192.168.X.1 | Firewall, routage, DHCP, Unbound, WireGuard | 4 |
| Proxmox Backup Server | Physique (ancien PC) | 40 ADMIN | 192.168.40.20 | Sauvegardes des VMs et LXC | 5 |
| Prometheus | LXC | 50 MONITORING | 192.168.50.10 | Collecte de métriques | 6 |
| Grafana | LXC | 50 MONITORING | 192.168.50.11 | Tableaux de bord | 6 |
| Wazuh | VM | 50 MONITORING | 192.168.50.20 | SIEM, détection d'intrusion | 7 |
| Kali Linux | VM | 60 LAB-ATTAQUE | 192.168.60.10 | Tests de sécurité | 7 |
| Pi-hole | LXC | 20 SERVICES | 192.168.20.53 | DNS filtrant | 8 |
| Vaultwarden | LXC | 20 SERVICES | 192.168.20.10 | Gestionnaire de mots de passe | 8 |
| Paperless-NGX | LXC | 20 SERVICES | 192.168.20.11 | Gestion de documents | 8 |
| Nextcloud | LXC/VM | 20 SERVICES | 192.168.20.12 | Fichiers | 8 |
| Immich | LXC/VM | 20 SERVICES | 192.168.20.13 | Photos | 8 |
| Homepage | LXC | 20 SERVICES | 192.168.20.15 | Tableau de bord des services | 8 |
| Gitea | LXC | 20 SERVICES | 192.168.20.14 | Dépôts Git, CI | 9 |

## Ressources

La RAM est estimée à la louche, pour dimensionner le serveur :

| Bloc | RAM indicative |
|---|---|
| OPNsense | 2 Go |
| Wazuh (all-in-one) | 6 à 8 Go |
| Prometheus + Grafana | 1 à 2 Go |
| Kali | 2 à 4 Go (allumée à la demande) |
| Services du quotidien (Nextcloud, Immich, Paperless, Vaultwarden, Pi-hole, Homepage) | 6 à 8 Go |
| Gitea | 0,5 Go |
| **Total** | **≈ 20 à 25 Go**, d'où l'objectif de 32 Go |
