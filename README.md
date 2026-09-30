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
| nginx | Sert mon portfolio (site statique) | Public (Cloudflare Tunnel) |
| Portainer | Interface de gestion des conteneurs Docker | Tailscale uniquement |

## Structure du dépôt

Chaque service a son propre dossier avec son `docker-compose.yml` :

\`\`\`
homelab/
├── nginx/
│   └── docker-compose.yml
└── portainer/
    └── docker-compose.yml
\`\`\`

## Pourquoi ce projet

Mise en pratique de concepts réseau/sécurité (VPN, reverse proxy, isolation de conteneurs, principe de moindre exposition) dans un environnement réel que j'administre de bout en bout.
