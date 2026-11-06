# Roadmap — Homelab

La progression se fait par étapes : on ne passe pas à une phase tant que la précédente ne fonctionne pas et n'est pas documentée. Les dates ne sont pas figées.

## Vue d'ensemble

| Phase | Intitulé | Statut |
|---|---|---|
| 1 | Matériel | En cours |
| 2 | Installation Proxmox | À faire (dépend du serveur) |
| 3 | Réseau et VLANs | Commencée |
| 4 | OPNsense | À faire |
| 5 | Sauvegardes | À faire |
| 6 | Supervision et observabilité | À faire |
| 7 | Services de sécurité | À faire |
| 8 | Services auto-hébergés du quotidien | À faire |
| 9 | Automatisation et DevSecOps | À faire |
| 10 | Plus tard : Azure, conteneurs et orchestration | Pas planifié |

## Priorités actuelles

1. Choisir le serveur principal ([critères](03-materiel.md))
2. Sécuriser le switch : management dans le VLAN 40, VLAN 1 inutilisé, ports inutilisés désactivés ([cible](../configs/switch/sw-lab.md))
3. Préparer l'ancien PC avec un disque dédié aux sauvegardes

---

## Phase 1 — Matériel

**Statut :** en cours

- [x] Switch Cisco WS-C2960-8TC-L
- [x] Câble console
- [x] Ancien PC 4 Go (futur PBS)
- [ ] Serveur principal : 2 ports réseau, 32 Go de RAM
- [ ] Disque dédié aux sauvegardes pour l'ancien PC

**Terminée quand :** tout le matériel de l'[inventaire](03-materiel.md) est disponible.

## Phase 2 — Installation Proxmox

- [ ] Installer Proxmox VE sur le serveur principal
- [ ] Configurer un bridge VLAN-aware sur le port trunk
- [ ] Mettre l'interface de gestion dans le VLAN 40 (192.168.40.10)
- [ ] Désactiver le dépôt entreprise, activer le dépôt no-subscription, mettre à jour

**Terminée quand :** l'interface web de Proxmox est joignable depuis le PC principal, dans le VLAN ADMIN.

## Phase 3 — Réseau et VLANs

- [x] Créer les VLANs 10 à 40 sur le switch
- [ ] Créer les VLANs 50, 60 et 999
- [ ] Déplacer le management du switch dans le VLAN 40, désactiver `Vlan1`
- [ ] Trunk sur Gi0/1, VLAN natif 999
- [ ] Désactiver les ports inutilisés et les placer dans le VLAN 999
- [ ] Vérifier la disponibilité de SSH (image k9), sinon restreindre Telnet
- [ ] Ajouter le `running-config` nettoyé dans `configs/switch/`

**Terminée quand :** `show vlan brief` et `show interfaces trunk` correspondent à la [configuration cible](../configs/switch/sw-lab.md).

## Phase 4 — OPNsense

- [ ] VM OPNsense, avec une interface WAN et une interface par VLAN
- [ ] DHCP sur LAN et LAB-ATTAQUE, Unbound comme résolveur
- [ ] Règles de firewall selon la [matrice des flux](06-flux.md)
- [ ] WireGuard, avec une seule redirection de port sur la box (51820/UDP)
- [ ] Tester chaque ligne de la matrice et noter les résultats dans le [journal](journal/)

**Terminée quand :** chaque flux autorisé passe, et un flux non listé est bloqué et journalisé.

## Phase 5 — Sauvegardes

- [ ] Installer Proxmox Backup Server sur l'ancien PC
- [ ] Sauvegardes quotidiennes chiffrées des VMs et LXC
- [ ] Copie hors site
- [ ] Premier test de restauration documenté

**Terminée quand :** une VM a été restaurée avec succès depuis PBS. Voir la [stratégie de sauvegarde](07-sauvegardes.md).

## Phase 6 — Supervision et observabilité

- [ ] Prometheus et Grafana en LXC dans le VLAN 50
- [ ] node_exporter sur Proxmox et PBS, SNMP sur le switch, métriques OPNsense
- [ ] Premiers tableaux de bord et alertes (disque plein, hôte injoignable, sauvegarde en échec)

## Phase 7 — Services de sécurité

- [ ] Wazuh dans le VLAN 50, avec des agents sur les machines principales
- [ ] Remontée des logs du firewall (règles de refus)
- [ ] Kali dans le VLAN 60, avec une première cible vulnérable dans le même VLAN
- [ ] Un scénario de détection documenté : une attaque lancée depuis Kali, l'alerte correspondante vue dans Wazuh

## Phase 8 — Services auto-hébergés du quotidien

À ne commencer qu'une fois la phase 5 terminée.

- [ ] Pi-hole
- [ ] Vaultwarden
- [ ] Paperless-NGX
- [ ] Nextcloud
- [ ] Immich
- [ ] Homepage

## Phase 9 — Automatisation et DevSecOps

- [ ] Gitea
- [ ] Ansible : mises à jour et durcissement de base des LXC
- [ ] Terraform (provider Proxmox) pour créer les LXC
- [ ] Sauvegarde automatique des configurations (switch, OPNsense) dans Git

## Phase 10 — Plus tard

Pistes à revoir quand le reste sera stable : Azure et AZ-900, Terraform en mode hybride local/cloud, k3s.
