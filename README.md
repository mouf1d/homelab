# Homelab

Infrastructure personnelle auto-hébergée sur une machine Debian, gérée via Docker Compose.
Projet d'apprentissage dans le cadre de mon parcours en cybersécurité/réseau.

## Architecture

- **Hôte** : Debian (serveur unique, Docker + Docker Compose)
- **Accès distant** : Tailscale (VPN mesh, WireGuard) — aucun port ouvert sur le routeur
- **Exposition publique** : Cloudflare Tunnel (connexion sortante uniquement, pas de port-forwarding)

## Philosophie réseau

Aucun port n'est ouvert sur le routeur. Deux méthodes d'accès :
- **Services privés** (admin, outils perso) → accessibles uniquement via Tailscale
- **Services publics** (portfolio) → exposés via Cloudflare Tunnel, qui établit une connexion sortante depuis le serveur vers Cloudflare (pas d'entrée ouverte à scanner depuis Internet)

Objectif : réduire au maximum la surface d'attaque exposée.

## Services

| Service | Rôle | Accès |
|---|---|---|
| nginx | Sert mon portfolio (site statique), logs custom pour capter l'IP réelle des visiteurs via Cloudflare | Public (Cloudflare Tunnel) |
| Portainer | Interface de gestion des conteneurs Docker | Tailscale uniquement |
| CrowdSec | Détection d'intrusion : analyse les logs nginx en temps réel, détecte scans/probing/tentatives d'exploit, dashboard via CrowdSec Console | Tailscale + [CrowdSec Console](https://app.crowdsec.net) |
| Cockpit | Monitoring système (CPU, RAM, disque, services systemd, logs). Installé nativement (pas containerisé) car nécessite un accès direct au système hôte | Tailscale |

## Structure du dépôt

```
homelab/
├── nginx/
│   ├── conf.d/           # config nginx custom (log format Cloudflare)
│   └── docker-compose.yml
├── portainer/
│   └── docker-compose.yml
└── crowdsec/
    └── docker-compose.yml
​```

Cockpit n'apparaît pas dans cette structure : installé en paquet système (`apt install cockpit`), pas géré par Docker Compose.

## Pourquoi ce projet

Mise en pratique de concepts réseau/sécurité (VPN, reverse proxy, isolation de conteneurs, détection d'intrusion, principe de moindre exposition) dans un environnement réel que j'administre de bout en bout.
