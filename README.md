# Homelab

This repository documents my personal homelab built on Proxmox.

The goal of this system is:
- self-hosted services
- clear ownership boundaries
- stable long-running services
- learning through real usage, not simulations

## Host
- Proxmox VE
- Single 1TB disk mounted at '/data' on the host

## Core Services
- NAS (privileged LXC): SMB file access + Tailscale
- Media Server (privileged LXC): Jellyfin, Radarr, Sonarr, Prowlarr, qBittorrent
- Game Server (unprivileged LXC): Terraria (tmodloader)

## Storage
- Host mounts physical disk -> '/data'
- LXCs bind-mount required subpaths form '/data'
- Ownership differs between privileged and unprivileged containers

## Networking
- All services are reachable locally
- Tailscale is used for remote access

Details and design decisions are documented in 'notes/'.
