# Home Lab - Stack changes

## Goals
- [ ] Clean up stack boundaries (vstack/dstack/nstack) so responsibilities are clear
- [ ] Remove unused/redundant services
- [ ] Improve backup/security posture where needed

## Tasks

### Media stacks (vstack/dstack)
- [ ] **Move Jellyseerr from dstack → vstack**
  - [ ] Remove  service from 
  - [ ] Add  service to 
  - [ ] Decide whether to:
    - [ ] keep config volume where it is (), or
    - [ ] move it under vstack for a cleaner split
  - [ ] Verify Jellyseerr → Sonarr/Radarr connectivity (API + base URLs)

- [ ] **Remove NZBGet (standardise on SABnzbd)**
  - [ ] In Radarr: Settings → Download Clients
    - [ ] Ensure SABnzbd is enabled + tested
    - [ ] Disable/remove NZBGet
  - [ ] In Sonarr: Settings → Download Clients
    - [ ] Ensure SABnzbd is enabled + tested
    - [ ] Disable/remove NZBGet
  - [ ] Remove  service from 
  - [ ] Bring dstack up and validate: downloads, imports, indexers, UI access

- [ ] **Homepage placement decision**
  - [ ] Decide: media-only dashboard (keep in dstack) vs general lab dashboard (move to nstack)
  - [ ] If using , treat Homepage as privileged; keep LAN-only or behind auth

### Network tools (nstack)
- [ ] **AdGuard Home sanity check**
  - [ ] Confirm volume mappings:
    - [ ] 
    - [ ] 
  - [ ] Confirm port exposure is intentional (DNS + UI)

### New: Document management
- [ ] **Trial paperless-ngx**
  - [ ] Decide host/runtime (Docker on server/NAS/VM)
  - [ ] Choose DB (PostgreSQL recommended)
  - [ ] Define storage/backup approach (docs + DB; consider encrypted backups to cloud)
  - [ ] Decide integration approach with Obsidian (link-by-ID + metadata frontmatter)

## Notes
- Keep image tags pinned for critical services (avoid  surprises)
- Consider explicit docker networks if you want service discovery across stacks without host ports
