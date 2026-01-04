# Homelab Overview

This document describes how services, storage, and permissions are structured.

This system is intentionally built horizontally:
- each major service runs in its own container
- failures are isolated

## High-Level Layout

Proxmox Host
└── /data (1TB disk)
    ├── NAS LXC (privileged)
    │   └── SMB exports + Tailscale
    ├── Media LXC (privileged)
    │   ├── Jellyfin
    │   ├── *arr apps
    │   └── qBittorrent
    └── Terraria LXC (unprivileged)
        └── tModLoader server

## Storage & Ownership Model

- The physical disk is mounted on the  Proxmox host at '/data'
- LXCs do not see the raw disk, only bin-mounted paths

### Privileged Containers
- NAS and Media containers run privileged
- They use host UID/GID mapping (1000:1000)
- This simplifies SMB and media access

### Unprivileged Container (Terraria)
- Terraria runs unprivileged
- Uses UID/Gid mapping (100000:100000)
- only '/data/games/terraria' is mounted
- This prevents accidental access to other data

## Design Decisions

- NAS is seperated from Media to keep file access and application logic isolated
- Media apps share a container to reduce permissiion complexity
- Game server is isolated to limit blast radius and simplify restarts
- Horizontal seperation is preffered over one large VM
