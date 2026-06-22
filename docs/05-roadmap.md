# Roadmap — Homelab

Ce document présente la feuille de route du homelab, avec une progression pensée pour construire une infrastructure cohérente, documentée et exploitable à moyen terme. L'objectif est d'avancer par étapes logiques, sans complexifier trop tôt l'environnement.

---

## Vue d'ensemble

| Phase | Intitulé | Statut | Finalité |
|---|---|---|---|
| 1 | Acquisition du matériel | Fait | Réunir la base matérielle du projet |
| 2 | Installation Proxmox | À faire | Mettre en place l'hyperviseur principal |
| 3 | Configuration réseau et VLANs | À faire | Structurer le réseau et isoler les usages |
| 4 | Déploiement OPNsense | À faire | Ajouter un firewall et une logique de routage avancée |
| 5 | Supervision et observabilité | À faire | Mesurer, surveiller et visualiser l'état du lab |
| 6 | Services de sécurité | À faire | Ajouter des outils orientés cybersécurité |
| 7 | DevSecOps et automatisation | À faire | Standardiser, automatiser et versionner |
| 8 | Services auto-hébergés du quotidien | À faire | Héberger des services réellement utiles |
| 9 | Cloud et Azure | À faire | Étendre le projet vers le cloud public |
| 10 | Conteneurisation et orchestration | À faire | Explorer Docker, k3s et éventuellement Kubernetes |

---

## Phase 1 — Acquisition du matériel

**Statut :** terminée

Cette phase consiste à réunir le matériel de base nécessaire pour démarrer le homelab dans de bonnes conditions.

### Éléments concernés

- Switch Cisco WS-C2960-8TC-L
- Ancien PC 4 Go RAM pour l'observabilité légère
- Raspberry Pi Zero 2 W prévu pour les services réseau permanents
- Mini PC principal encore à acheter pour Proxmox

### Résultat attendu

À l'issue de cette phase, la base matérielle est identifiée et l'architecture générale devient réaliste à mettre en œuvre.

---

## Phase 2 — Installation Proxmox

**Statut :** à faire

Cette phase pose les fondations de l'infrastructure de virtualisation. Le mini PC principal doit devenir le nœud central du homelab.

### Tâches principales

- Choisir et acheter le mini PC principal
- Installer Proxmox VE
- Configurer le stockage local initial
- Vérifier l'accès d'administration de base
- Préparer les premières VMs et LXC

### Résultat attendu

Un hyperviseur principal fonctionnel, prêt à accueillir les services et l'architecture réseau du projet.

---

## Phase 3 — Configuration réseau et VLANs

**Statut :** à faire

Cette phase a pour but de structurer proprement le réseau du lab afin d'éviter un environnement plat et peu réaliste.

### Tâches principales

- Configurer le switch Cisco
- Définir les VLANs principaux : LAN, LAB, DMZ, ADMIN
- Préparer les trunks et ports d'accès nécessaires
- Documenter l'adressage logique
- Vérifier la séparation effective des flux

### Résultat attendu

Un réseau segmenté, plus propre, plus pédagogique et plus proche d'un environnement professionnel.

---

## Phase 4 — Déploiement OPNsense

**Statut :** à faire

Cette phase introduit le composant firewall / routeur central du lab. OPNsense permettra de travailler la sécurité, le routage et les règles réseau de façon plus réaliste.

### Tâches principales

- Déployer une VM OPNsense sur Proxmox
- Configurer les interfaces réseau et VLANs
- Mettre en place les règles firewall de base
- Préparer éventuellement les accès VPN côté firewall
- Tester le routage entre segments selon la politique choisie

### Résultat attendu

Un point de contrôle réseau centralisé, capable de segmenter et filtrer les flux du homelab.

---

## Phase 5 — Supervision et observabilité

**Statut :** à faire

Cette phase exploite l'ancien PC 4 Go RAM pour héberger une base légère d'observabilité.

### Tâches principales

- Installer Prometheus
- Installer Grafana
- Ajouter les exporters nécessaires
- Construire les premiers dashboards
- Surveiller l'état des machines et des services essentiels

### Résultat attendu

Une visibilité concrète sur l'état du lab, sa disponibilité et ses ressources.

---

## Phase 6 — Services de sécurité

**Statut :** à faire

Cette phase introduit les briques orientées cybersécurité, dans une logique de pratique réelle et de montée en compétence.

### Tâches principales

- Déployer Wazuh
- Déployer une VM Kali Linux
- Centraliser les logs utiles
- Tester des scénarios simples de détection
- Documenter les premiers cas d'usage sécurité

### Résultat attendu

Un environnement initial de détection, de journalisation et de test orienté sécurité.

---

## Phase 7 — DevSecOps et automatisation

**Statut :** à faire

Cette phase vise à rendre l'infrastructure plus propre, reproductible et maintenable.

### Tâches principales

- Déployer GitLab ou Gitea
- Structurer les dépôts d'infrastructure
- Introduire Ansible pour la configuration
- Introduire Terraform pour l'infrastructure as code
- Préparer une logique CI/CD adaptée au projet

### Résultat attendu

Un homelab mieux documenté, versionné et automatisé, avec une vraie dimension portfolio.

---

## Phase 8 — Services auto-hébergés du quotidien

**Statut :** à faire

Cette phase donne au projet une utilité concrète au-delà de l'apprentissage pur.

### Services envisagés

- Vaultwarden
- Paperless-NGX
- Nextcloud
- Immich
- Homepage ou Homarr
- Portainer

### Résultat attendu

Une infrastructure qui rend des services réellement utiles au quotidien, en plus de servir de terrain d'apprentissage.

---

## Phase 9 — Cloud et Azure

**Statut :** à faire

Cette phase prolonge le homelab vers le cloud public et soutient la préparation des certifications visées.

### Tâches principales

- Travailler les bases Azure
- Relier certains concepts du lab au cloud public
- Utiliser Terraform dans un contexte mixte local / cloud
- Préparer les objectifs liés à AZ-900 puis éventuellement au-delà

### Résultat attendu

Une meilleure continuité entre compétences homelab, cloud et certification.

---

## Phase 10 — Conteneurisation et orchestration

**Statut :** à faire

Cette phase sert à aller plus loin sur les usages modernes d'infrastructure applicative.

### Tâches principales

- Déployer des services conteneurisés de façon plus systématique
- Structurer les usages Docker
- Tester k3s comme première approche légère
- Évaluer plus tard Kubernetes ou AKS selon la progression du projet

### Résultat attendu

Une ouverture vers des architectures plus modernes et une progression naturelle vers l'orchestration.

---

## Priorités actuelles

Les priorités les plus logiques à court terme sont les suivantes :

1. Acheter le mini PC principal
2. Installer Proxmox
3. Mettre en place le Raspberry Pi Zero 2 W
4. Monter la base réseau avec les VLANs
5. Activer Prometheus et Grafana sur l'ancien PC

---

## Notes

- Les dates ne sont pas figées : cette roadmap décrit un ordre logique plus qu'un calendrier strict.
- Certaines phases peuvent se chevaucher partiellement selon le matériel disponible et le temps consacré au projet.
- Les services listés représentent une cible de moyen terme ; tous ne seront pas forcément déployés immédiatement.
