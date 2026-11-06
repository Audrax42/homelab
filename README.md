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

## Roadmap

- [x] Phase 1 — Acquisition du matériel
- [ ] Phase 2 — Installation Proxmox
- [ ] Phase 3 — Configuration réseau et VLANs
- [ ] Phase 4 — Déploiement OPNsense
- [ ] Phase 5 — Supervision et observabilité
- [ ] Phase 6 — Services de sécurité
- [ ] Phase 7 — DevSecOps et automatisation
- [ ] Phase 8 — Services auto-hébergés du quotidien
- [ ] Phase 9 — Cloud et Azure
- [ ] Phase 10 — Conteneurisation et orchestration

---

## Matériel

| Équipement | Modèle | Rôle |
|---|---|---|
| Switch | Cisco WS-C2960-8TC-L | Commutation réseau, VLANs |
| Serveur principal cible | Mini PC à acheter, minimum 16 Go RAM / 4 cœurs | Hyperviseur Proxmox principal |
| Ancien PC | Vieux PC 4 Go RAM | Hébergement léger, probablement Prometheus + Grafana uniquement |
| Micro-serveur | Raspberry Pi Zero 2 W (prévu) | WireGuard, Pi-hole, WoL |

---

## Architecture réseau

L'architecture cible repose sur un switch managé Cisco, un hyperviseur Proxmox principal, un micro-serveur léger toujours allumé, et une segmentation logique par VLANs.

> Les adresses IP affichées ci-dessous sont génériques et ne correspondent pas forcément aux adresses réellement retenues dans l'infrastructure finale. Elles sont utilisées ici à titre documentaire pour des raisons de lisibilité et de sécurité.

```
Internet
   |
[Box Internet]
   |
[Switch Cisco 2960]
   |
   ├── [Mini PC Proxmox]
   │      |
   │      ├── [OPNsense VM] ── Firewall + Routeur
   │      |
   │      ├── VLAN 10 — LAN   192.168.10.0/24
   │      ├── VLAN 20 — LAB   192.168.20.0/24
   │      ├── VLAN 30 — DMZ   192.168.30.0/24
   │      └── VLAN 40 — ADMIN 192.168.40.0/24
   |
   ├── [Ancien PC 4 Go]
   │      └── Prometheus / Grafana
   |
   └── [Raspberry Pi Zero 2 W]
          ├── WireGuard
          ├── Pi-hole
          └── Wake-on-LAN
```

---

## Services prévus

| Service | Rôle | Emplacement prévu |
|---|---|---|
| OPNsense | Firewall, routage, segmentation réseau, VPN éventuel | Proxmox |
| WireGuard | VPN d'accès distant sécurisé au réseau domestique | Raspberry Pi |
| Pi-hole | Blocage des publicités et trackers sur le réseau | Raspberry Pi |
| Wake-on-LAN | Réveil à distance du mini PC Proxmox | Raspberry Pi |
| Prometheus | Collecte de métriques et supervision | Ancien PC 4 Go |
| Grafana | Visualisation des métriques et tableaux de bord | Ancien PC 4 Go |
| Wazuh | SIEM, centralisation des logs et détection | Proxmox |
| Kali Linux | Machine de test sécurité et exercices offensifs | Proxmox |
| GitLab / Gitea | Dépôts Git, CI/CD, centralisation de projets | Proxmox |
| Vaultwarden | Gestionnaire de mots de passe auto-hébergé | Proxmox |
| Paperless-NGX | Gestion, archivage et recherche de documents | Proxmox |
| Nextcloud | Synchronisation et partage de fichiers | Proxmox |
| Immich | Sauvegarde et gestion des photos | Proxmox |
| Portainer | Gestion des conteneurs Docker | Proxmox |
| Homepage / Homarr | Tableau de bord central des services | Proxmox |
| Ansible | Automatisation de configuration | Poste d'administration / Proxmox |
| Terraform | Infrastructure as Code pour les futurs environnements | Poste d'administration / Proxmox |

---

## Compétences développées

- Virtualisation : Proxmox, VMs, LXC, snapshots
- Réseau : VLANs, routage inter-VLAN, firewall, VPN, DNS
- Cybersécurité : segmentation, SIEM, durcissement, logs
- Automatisation : Terraform, Ansible, CI/CD GitLab
- Cloud : Azure, Terraform, AZ-900, AZ-500
- Administration système : Linux, SSH, services, sauvegardes
- Observabilité : Prometheus, Grafana, métriques, supervision

---

## Documentation

- [Architecture détaillée](docs/01-architecture.md)
- [Plan d'adressage IP](docs/02-adressage-ip.md)
- [Inventaire matériel](docs/03-materiel.md)
- [Liste des VMs](docs/04-vms.md)
- [Roadmap détaillée](docs/05-roadmap.md)
