# Homelab — Infrastructure personnelle

Projet d'infrastructure homelab construit pour développer des compétences concrètes en réseau, cybersécurité, virtualisation et DevSecOps, tout en hébergeant des services utiles au quotidien.

---

## Objectifs

- Apprendre et pratiquer sur une infrastructure réelle
- Préparer les certifications CCNA, Security+, AZ-900
- Construire un portfolio technique visible pour les recruteurs
- Héberger des services utiles au quotidien
- Concevoir une infrastructure sobre en énergie avec accès distant sécurisé

---

## État actuel

| Élément | État |
|---|---|
| Switch Cisco | VLANs créés, sécurisation et trunk à refaire |
| Serveur principal | Pas encore choisi |
| Ancien PC (sauvegardes) | Disponible |
| Proxmox, OPNsense, services | Pas encore déployés |

---

## Roadmap

- [ ] Phase 1 — Matériel *(en cours)*
- [ ] Phase 2 — Installation Proxmox
- [ ] Phase 3 — Réseau et VLANs *(commencée)*
- [ ] Phase 4 — Déploiement OPNsense
- [ ] Phase 5 — Sauvegardes
- [ ] Phase 6 — Supervision et observabilité
- [ ] Phase 7 — Services de sécurité
- [ ] Phase 8 — Services auto-hébergés du quotidien
- [ ] Phase 9 — Automatisation et DevSecOps
- [ ] Phase 10 — Plus tard : Azure, conteneurs et orchestration

Détail : [roadmap](docs/05-roadmap.md)

---

## Matériel

| Équipement | Modèle | Rôle |
|---|---|---|
| Switch | Cisco WS-C2960-8TC-L | Commutation, VLANs |
| Serveur principal | À définir (2 ports réseau, 32 Go de RAM visés) | Proxmox + OPNsense, toujours allumé |
| Ancien PC | PC 4 Go RAM | Proxmox Backup Server |

---

## Architecture réseau

```
Internet
   |
[Box Internet]
   |
[Serveur Proxmox] ── OPNsense VM (firewall, routage, VPN WireGuard)
   |  trunk (Gi0/1)
[Switch Cisco 2960]
   |
   ├── VLAN 10 — LAN          192.168.10.0/24   postes perso
   ├── VLAN 20 — SERVICES     192.168.20.0/24   services auto-hébergés
   ├── VLAN 30 — DMZ          192.168.30.0/24   vide pour l'instant
   ├── VLAN 40 — ADMIN        192.168.40.0/24   gestion (Proxmox, switch, PBS)
   ├── VLAN 50 — MONITORING   192.168.50.0/24   Prometheus, Grafana, Wazuh
   └── VLAN 60 — LAB-ATTAQUE  192.168.60.0/24   Kali, isolé
```

- Tout passage d'un VLAN à un autre traverse OPNsense, avec un refus par défaut ([matrice des flux](docs/06-flux.md)).
- Rien n'est exposé sur Internet : l'accès à distance passe par WireGuard.
- Les sauvegardes sont faites sur une machine séparée ([stratégie de sauvegarde](docs/07-sauvegardes.md)).

---

## Services prévus

| Service | Rôle | VLAN |
|---|---|---|
| OPNsense | Firewall, routage, DHCP, DNS récursif, VPN WireGuard | Tous |
| Proxmox Backup Server | Sauvegardes des VMs et LXC | ADMIN |
| Prometheus / Grafana | Métriques et tableaux de bord | MONITORING |
| Wazuh | SIEM, centralisation des logs, détection | MONITORING |
| Kali Linux | Tests de sécurité | LAB-ATTAQUE |
| Pi-hole | DNS filtrant | SERVICES |
| Vaultwarden | Gestionnaire de mots de passe | SERVICES |
| Paperless-NGX | Gestion de documents | SERVICES |
| Nextcloud | Synchronisation de fichiers | SERVICES |
| Immich | Photos | SERVICES |
| Homepage | Tableau de bord des services | SERVICES |
| Gitea | Dépôts Git, CI | SERVICES |
| Ansible / Terraform | Automatisation, infrastructure as code | Poste d'administration |

---

## Compétences développées

- Virtualisation : Proxmox, VMs, LXC, snapshots
- Réseau : VLANs, trunks, routage inter-VLAN, firewall, VPN, DNS
- Cybersécurité : segmentation, matrice de flux, SIEM, durcissement, logs
- Sauvegarde : stratégie 3-2-1, PBS, tests de restauration
- Automatisation : Ansible, Terraform, CI
- Cloud : Azure, AZ-900
- Observabilité : Prometheus, Grafana

---

## Documentation

- [Architecture](docs/01-architecture.md)
- [Plan d'adressage IP](docs/02-adressage-ip.md)
- [Inventaire matériel](docs/03-materiel.md)
- [Liste des VMs et LXC](docs/04-vms.md)
- [Roadmap détaillée](docs/05-roadmap.md)
- [Matrice des flux](docs/06-flux.md)
- [Stratégie de sauvegarde](docs/07-sauvegardes.md)
- [Journal de bord](docs/journal/)
- [Configuration du switch](configs/switch/sw-lab.md)
