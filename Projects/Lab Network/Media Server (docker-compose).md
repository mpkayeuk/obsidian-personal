# Media Server (docker-compose)

## Repo
- Local: `~/projects/media-server`
- GitHub: https://github.com/The-Archers/docker-compose-mediaserver

This repo contains two compose stacks:
- `vstack/` — playback server (Jellyfin)
- `dstack/` — download + automation + dashboard tooling

---

## vstack (Jellyfin)
File: `vstack/docker-compose.yml`

### Service: jellyfin
Image: `lscr.io/linuxserver/jellyfin:latest`

Ports:
- `8096/tcp` — HTTP
- `8920/tcp` — HTTPS (if enabled/configured)
- `7359/udp` — DLNA discovery / auto-discovery
- `1900/udp` — SSDP

Volumes:
- `/Volumes/Evo/media-server/vstack/config/jellyfin` → `/config`
- `/Volumes/External/media/tv` → `/data/tv`
- `/Volumes/External/media/movies` → `/data/movies`

Run:
```bash
cd ~/projects/media-server/vstack
docker compose up -d
```

---

## dstack (downloads + automation)
File: `dstack/docker-compose.yml`

### Services
- **sabnzbd** (Usenet downloader)
  - port `8090` → container `8080`
  - volumes:
    - `/Volumes/Evo/media-server/dstack/config/sabnzbd` → `/config`
    - `/Volumes/Evo/media-server/dstack/downloads` → `/downloads`
    - `/Volumes/External/media/.adult` → `/media/adult`
  - notable env:
    - `SCRIPTS_DIR