# Homelab — Infrastructure personnelle

Projet d'infrastructure homelab construit pour développer des compétences
concrètes en réseau, cybersécurité, virtualisation et DevSecOps.

---

## Objectifs

- Apprendre et pratiquer sur une infrastructure réelle
- Préparer les certifications CCNA, Security+, AZ-900
- Construire un portfolio technique visible pour les recruteurs
- Héberger des services utiles au quotidien

---

## Matériel

| Équipement | Modèle | Rôle |
|---|---|---|
| Switch | Cisco WS-C2960-8TC-L | Commutation réseau, VLANs |
| Serveur principal | PC en cours de réparation | Hyperviseur Proxmox |
| Micro-serveur | Raspberry Pi Zero 2 W (prévu) | WireGuard, Pi-hole, WoL |
| Routeur | On Networks (ancien) | Réseau de test |

---

## Architecture réseau
Internet
|
[Box Internet]
|
[Switch Cisco 2960]
|
[PC Proxmox]
|
[OPNsense VM] ── Firewall + Routeur
|
├── VLAN 10 — LAN 192.168.10.0/24
├── VLAN 20 — LAB 192.168.20.0/24
├── VLAN 30 — DMZ 192.168.30.0/24
└── VLAN 40 — ADMIN 192.168.40.0/24

---

## Services prévus

| Service | Rôle | VLAN |
|---|---|---|
| OPNsense | Firewall, routage, VPN | — |
| Wazuh | SIEM, détection d'intrusion | LAB |
| Kali Linux | Tests de sécurité | LAB |
| GitLab / Gitea | Dépôts Git, CI/CD | LAB |
| Vaultwarden | Gestionnaire de mots de passe | ADMIN |
| Paperless-NGX | Gestion de documents | ADMIN |
| WireGuard | VPN accès distant | — |
| Pi-hole | Blocage pubs et trackers | — |

---

## Roadmap

- [x] Phase 1 — Acquisition du matériel
- [ ] Phase 2 — Installation Proxmox
- [ ] Phase 3 — Configuration réseau et VLANs
- [ ] Phase 4 — Déploiement OPNsense
- [ ] Phase 5 — Services de sécurité (Wazuh, Kali)
- [ ] Phase 6 — DevSecOps (GitLab, Terraform, Ansible)
- [ ] Phase 7 — Cloud (Azure, AZ-900)
- [ ] Phase 8 — Kubernetes (k3s, AKS)

---

## Compétences développées

- Virtualisation : Proxmox, VMs, LXC, snapshots
- Réseau : VLANs, routage inter-VLAN, firewall, VPN, DNS
- Cybersécurité : segmentation, SIEM, durcissement, logs
- Automatisation : Terraform, Ansible, CI/CD GitLab
- Cloud : Azure, Terraform, AZ-900, AZ-500
- Administration système : Linux, SSH, services, sauvegardes

---

## Documentation

- [Architecture détaillée](docs/01-architecture.md)
- [Plan d'adressage IP](docs/02-adressage-ip.md)
- [Inventaire matériel](docs/03-materiel.md)
- [Liste des VMs](docs/04-vms.md)
- [Roadmap détaillée](docs/05-roadmap.md)
