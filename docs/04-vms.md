# Liste des VMs et LXC

| Service | Type | VLAN | Rôle |
|---|---|---|---|
| OPNsense | VM | — | Firewall, routeur, VPN, DHCP |
| Wazuh | VM/LXC | LAB | SIEM, détection d'intrusion |
| Kali Linux | VM | LAB | Tests de sécurité |
| Ubuntu Server | LXC | LAB | Administration générale |
| GitLab/Gitea | LXC | LAB | Dépôts Git, CI/CD |
| Vaultwarden | LXC | ADMIN | Gestionnaire mots de passe |
| Paperless-NGX | LXC | ADMIN | Gestion documents |
| Pi-hole | LXC/Pi | — | Blocage pubs et trackers |