---
tags: [server, linux, homelab]
---

# 10 – Næste skridt

← [[12 - Cron job - Jobsøgning]] | Fejlfinding: [[11 - Fejlfinding]]

Grundopsætningen er færdig. Her er idéer til, hvad der er værd at lave bagefter, med de vigtigste først.

## 1. Backup (vigtigst!)

> En server uden backup er en server, der venter på at miste data.

- **Hvad skal med:** `~/docker` (compose-filer + data-mapper), `/etc` og evt. `~/.ssh`.
- **Værktøj:** [restic](https://restic.net) eller [BorgBackup](https://www.borgbackup.org) – krypterede, inkrementelle backups.
- **Hvorhen:** en ekstern disk + et sted uden for huset (fx Backblaze B2, Hetzner Storage Box eller en vens server via Tailscale).
- Kør backup automatisk med en `systemd`-timer eller `cron`, og **test en gendannelse** mindst én gang.

```bash
sudo apt install -y restic
```

## 2. Overvågning

- **Uptime Kuma** – er mine tjenester oppe? (eksemplet i [[08 - Docker]])
- **btop** – live CPU/RAM/disk i terminalen.
- **Beszel** eller **Netdata** – grafer over ressourceforbrug over tid.
- Diskens helbred:
  ```bash
  sudo apt install -y smartmontools
  sudo smartctl -a /dev/nvme0n1
  ```

## 3. Administration via web (valgfrit)

- **Dockge** – nemt web-UI til at styre compose-stacks i `~/docker`.
- **Portainer** – mere omfattende Docker-UI.
- **Cockpit** – web-UI til selve Ubuntu (logs, services, opdateringer):
  ```bash
  sudo apt install -y cockpit
  # https://homeserver:9090
  ```

## 4. Reverse proxy + pæne adresser

Når der er flere tjenester, kan jeg få adresser som `kuma.homeserver.ts.net` i stedet for `homeserver:3001`:

- **Caddy** eller **Nginx Proxy Manager** som reverse proxy.
- **Tailscale Serve** – giver HTTPS på tailnet-adressen med én kommando:
  ```bash
  sudo tailscale serve --bg 3001
  ```

## 5. Populære tjenester til en hjemmeserver

| Tjeneste | Hvad |
|----------|------|
| **Jellyfin** | Egen Netflix til film og serier |
| **Immich** | Billedbackup fra telefonen (Google Photos-alternativ) |
| **Nextcloud** / **Seafile** | Filer og kalender |
| **Vaultwarden** | Password manager (Bitwarden-kompatibel) |
| **AdGuard Home** / **Pi-hole** | Reklameblokering for hele netværket |
| **Home Assistant** | Smart home |
| **Gitea** / **Forgejo** | Egen Git-server |
| **Syncthing** | Synk mapper mellem enheder (fx min Obsidian-vault) |
| **Watchtower** / **What's Up Docker** | Opdatér / giv besked om nye images |

## 6. Vedligeholdelse – rutine

| Hvor tit | Opgave |
|----------|--------|
| Automatisk | Sikkerhedsopdateringer (unattended-upgrades) |
| Ugentligt | `update` (alias), tjek `df -h`, tjek at backup kørte |
| Månedligt | `docker compose pull && docker compose up -d` i hver mappe, `docker image prune -a` |
| Hvert halve år | Test gendannelse af backup |
| Hvert 2. år | `sudo do-release-upgrade` til næste LTS (vent til `.1`-udgaven) |
