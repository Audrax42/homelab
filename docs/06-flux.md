# Matrice des flux

Ce document définit **qui a le droit de parler à qui**. Les règles du firewall OPNsense en découlent directement : tout flux absent de ce document est refusé.

## Règles générales

1. **Refus par défaut** entre VLANs. Seuls les flux listés ci-dessous sont autorisés.
2. Le trafic à l'intérieur d'un même VLAN ne passe pas par le firewall (il reste au niveau 2, sur le switch ou le bridge Proxmox).
3. Aucun VLAN n'a accès aux interfaces de gestion d'OPNsense (GUI, SSH), sauf ADMIN et le PC principal.
4. Tous les VLANs ont accès aux services de base de leur propre passerelle : DHCP (si activé), NTP (123/UDP) et ICMP.
5. Les règles qui refusent sont journalisées, pour alimenter Wazuh.

## Vue d'ensemble

Lecture : la **ligne** est la source, la **colonne** la destination.

| Source ↓ / Destination → | Internet | LAN | SERVICES | DMZ | ADMIN | MONITORING | LAB-ATTAQUE |
|---|---|---|---|---|---|---|---|
| **LAN** (10) | ✅ | — | ⚠️ web + DNS | ⚠️ web | ❌ ¹ | ❌ ¹ | ❌ |
| **SERVICES** (20) | ⚠️ mises à jour | ❌ | — | ❌ | ❌ | ⚠️ logs | ❌ |
| **DMZ** (30) | ⚠️ mises à jour | ❌ | ⚠️ backends ² | — | ❌ | ⚠️ logs | ❌ |
| **ADMIN** (40) | ⚠️ mises à jour | ✅ | ✅ | ✅ | — | ✅ | ✅ |
| **MONITORING** (50) | ⚠️ mises à jour | ⚠️ métriques | ⚠️ métriques | ⚠️ métriques | ⚠️ métriques | — | ⚠️ métriques |
| **LAB-ATTAQUE** (60) | ⚠️ filtré | ❌ | ❌ | ❌ | ❌ | ❌ | — |
| **VPN** (WireGuard) | ✅ | ❌ | ⚠️ web + DNS | ❌ | ❌ ³ | ❌ ³ | ❌ |

✅ autorisé · ⚠️ autorisé de façon restreinte (détail ci-dessous) · ❌ refusé

¹ Exception : le PC principal (192.168.10.10). Voir le détail.
² Seulement quand un service sera exposé, et uniquement du reverse proxy vers les backends concernés.
³ Exception : le pair VPN « admin ». Voir le détail.

## Détail des flux autorisés

### Depuis LAN (192.168.10.0/24)

| Destination | Ports | Raison |
|---|---|---|
| Internet | Tous | Navigation |
| Pi-hole 192.168.20.53 | 53/TCP+UDP | DNS |
| SERVICES | 443/TCP | Accès web aux services |
| DMZ | 443/TCP | Plus tard, si un service y est placé |

### Exception : PC principal (192.168.10.10)

| Destination | Ports | Raison |
|---|---|---|
| ADMIN | 22/TCP, 443/TCP, 8006/TCP (Proxmox), 8007/TCP (PBS) | Administration |
| Switch 192.168.40.2 | 22/TCP (ou 23/TCP tant que SSH n'est pas disponible) | Administration du switch |
| MONITORING | 3000/TCP (Grafana), 443/TCP (Wazuh) | Consultation des tableaux de bord |
| LAB-ATTAQUE | 22/TCP | Accès à la VM Kali |

### Depuis SERVICES (192.168.20.0/24)

| Destination | Ports | Raison |
|---|---|---|
| Internet | 80/TCP, 443/TCP | Mises à jour, images de conteneurs |
| Unbound (passerelle) | 53/TCP+UDP | Pi-hole → résolveur récursif |
| Wazuh 192.168.50.20 | 1514/TCP, 1515/TCP | Envoi des logs, enrôlement des agents |

### Depuis ADMIN (192.168.40.0/24)

| Destination | Ports | Raison |
|---|---|---|
| Tous les VLANs | Tous | Administration |
| Internet | 80/TCP, 443/TCP | Mises à jour |

### Depuis MONITORING (192.168.50.0/24)

| Destination | Ports | Raison |
|---|---|---|
| Tous les VLANs | 9100/TCP (node_exporter), ICMP | Collecte des métriques |
| Switch 192.168.40.2 | 161/UDP (SNMP) | Métriques du switch |
| OPNsense | 9100/TCP ou API | Métriques du firewall |
| Internet | 80/TCP, 443/TCP | Mises à jour, règles Wazuh |

### Depuis LAB-ATTAQUE (192.168.60.0/24)

| Destination | Ports | Raison |
|---|---|---|
| Internet | 80/TCP, 443/TCP | Mises à jour Kali, outils |
| Unbound (passerelle) | 53/TCP+UDP | DNS, sans passer par SERVICES |

Aucun autre flux. Les exercices offensifs se font **uniquement sur des cibles placées dans ce même VLAN**.

### Depuis le VPN WireGuard (192.168.100.0/24)

| Pair | Destination | Ports | Raison |
|---|---|---|---|
| Téléphone, portable | Pi-hole, SERVICES | 53, 443/TCP | Accès aux services à distance |
| Pair « admin » | Mêmes droits que le PC principal | — | Administration à distance |

### Vers le WAN (entrant depuis Internet)

| Destination | Ports | Raison |
|---|---|---|
| OPNsense (WAN) | 51820/UDP | WireGuard |

Rien d'autre n'est ouvert en entrant. Sur la box, une seule redirection de port est configurée : 51820/UDP vers OPNsense.

## Vérification

Une fois les règles en place, chaque ligne de ce document doit être testée : depuis la source, on vérifie que le flux autorisé passe et qu'un flux non listé est bloqué (avec `nc`, `curl`, `nmap` et les logs du firewall). Les résultats sont notés dans le [journal](journal/).
