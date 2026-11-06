# Stratégie de sauvegarde

Le homelab hébergera des données qu'on ne peut pas perdre : mots de passe (Vaultwarden), documents (Paperless), photos (Immich), fichiers (Nextcloud). Les sauvegardes sont donc en place **avant** les services du quotidien (phase 5, avant la phase 8).

## Règle 3-2-1

| Copie | Où | Support |
|---|---|---|
| 1 — Production | Serveur Proxmox | SSD NVMe |
| 2 — Sauvegarde locale | Proxmox Backup Server (ancien PC) | Disque dédié |
| 3 — Hors site | Disque externe rangé ailleurs, ou stockage cloud | Chiffré |

## Ce qui est sauvegardé

| Élément | Méthode | Fréquence |
|---|---|---|
| VMs et LXC | Sauvegarde Proxmox vers PBS | Quotidienne |
| Configuration OPNsense | Export XML (System → Configuration → Backups) | À chaque modification + hebdomadaire |
| Configuration du switch | `show running-config`, version nettoyée dans `configs/switch/` | À chaque modification |
| Configuration de l'hôte Proxmox | `/etc/pve`, `/etc/network/interfaces` | Hebdomadaire |
| Vaultwarden | Export chiffré en plus de la sauvegarde du LXC | Hebdomadaire |

## Planification et rétention

- Sauvegarde quotidienne vers 2 h du matin
- Rétention PBS : 7 quotidiennes, 4 hebdomadaires, 6 mensuelles
- Vérification automatique des sauvegardes (*verify jobs*) une fois par semaine
- Copie hors site : au minimum une fois par mois

## Économie d'énergie

L'ancien PC n'a pas besoin d'être allumé en permanence :
1. OPNsense le réveille par Wake-on-LAN peu avant la fenêtre de sauvegarde
2. Les sauvegardes et vérifications s'exécutent
3. Il s'éteint automatiquement une fois les tâches terminées

## Chiffrement et clés

- Les sauvegardes PBS sont **chiffrées côté client**.
- La clé de chiffrement et le mot de passe maître de Vaultwarden sont conservés **en dehors du homelab** (sur papier, dans un endroit sûr). Si ces clés n'étaient que dans Vaultwarden, une panne totale du lab les rendrait inaccessibles en même temps que les sauvegardes.

## Tests de restauration

Une sauvegarde qui n'a jamais été restaurée n'a pas valeur de preuve.

- **Chaque trimestre** : restaurer un LXC choisi au hasard sur un ID temporaire, vérifier que le service démarre et que ses données sont là, puis le supprimer.
- **Une fois par an** : simuler une perte totale de l'hôte et noter le temps nécessaire pour tout remonter.
- Chaque test est consigné dans le [journal](journal/).
