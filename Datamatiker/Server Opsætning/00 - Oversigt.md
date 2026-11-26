---
tags: [server, linux, homelab]
oprettet: 2026-11-26
---

# Server Opsætning – Oversigt

Guide til at sætte min mini PC op som hjemmeserver med **Ubuntu Server (LTS)**. Skærm og tastatur bruges kun i starten; så hurtigt som muligt går jeg over til at logge ind med **SSH fra min primære PC** og laver resten derfra.

## Rækkefølge

Rækkefølgen er valgt sådan, at jeg kan tage skærmen af så tidligt som muligt, og sådan at hvert trin bygger på det forrige.

| # | Trin | Skærm på? | Hvorfor her |
|---|------|-----------|-------------|
| 1 | [[01 - Forberedelse og USB]] | – | BIOS-indstillinger og installations-USB laves på forhånd |
| 2 | [[02 - Installer Ubuntu Server]] | ✅ | Installationen kræver skærm og tastatur |
| 3 | [[03 - Første boot og SSH]] | ✅ → ❌ | Når SSH virker, kan skærmen tages af |
| 4 | [[04 - Herdr]] | ❌ | Faste sessioner – resten af opsætningen laves inde i herdr, så intet stopper, hvis SSH ryger |
| 5 | [[05 - Grundopsætning og sikkerhed]] | ❌ | Opdateringer, firewall og SSH-hærdning før der installeres andet |
| 6 | [[06 - Grundværktøjer og Git]] | ❌ | git, btop, ncdu m.m. bruges i resten af guiden |
| 7 | [[07 - Teksteditor (micro)]] | ❌ | En editor jeg kan lide, før jeg skal redigere konfigurationsfiler |
| 8 | [[08 - Tailscale]] | ❌ | Adgang til serveren hjemmefra og udefra uden at åbne porte i routeren |
| 9 | [[09 - Docker]] | ❌ | Docker kører alle tjenesterne på serveren |
| 10 | [[10 - Claude Code]] | ❌ | Kommer sidst, så Claude kan hjælpe med vedligehold og nye tjenester |
| 11 | [[11 - Cron job - Jobsøgning]] | ❌ | Kører `job-soegning`-skill'en én gang om ugen om natten |
| 12 | [[12 - Cron job - Auto pull af git repos]] | ❌ | `git pull` i alle repos hver nat |
| 13 | [[13 - Næste skridt]] | ❌ | Backup, overvågning og forslag til tjenester |

Fejlfinding: [[14 - Fejlfinding]]

Ekstra enheder: [[15 - Forbind fra laptop]] – kan laves når som helst efter trin 8

> [!tip] Hvorfor Claude Code til sidst?
> Claude Code har mest værdi, når git, Docker og resten allerede er installeret. Så kan den arbejde direkte med `docker compose`, repos og logfiler. Den kan godt installeres tidligere, hvis jeg vil have hjælp undervejs.

## Hvad jeg har tilføjet ud over min oprindelige liste

- **OpenSSH server** – installeres sammen med Ubuntu, så jeg hurtigt kan komme på fra min primære PC.
- **UFW (firewall)** og **unattended-upgrades** (automatiske sikkerhedsopdateringer).
- **herdr** – faste terminal-sessioner til AI-agenter. Processer (fx Claude Code) bliver ved med at køre, selvom SSH-forbindelsen ryger. **tmux** som reserve.
- **btop, ncdu, curl, unzip** – overvågning og diverse værktøjer.
- **micro** som teksteditor – tastaturgenveje som i VS Code (Ctrl+S, Ctrl+C/V, Ctrl+F, mus).
- **VS Code Remote-SSH** (valgfrit) – mappen på serveren åbnes i VS Code på min primære PC.
- **gh** (GitHub CLI) – valgfrit, nemt login på GitHub.
- **Cron jobs** – ugentlig jobsøgning med Claude Code og `job-soegning`-skill'en, og natlig auto pull af alle git repos.

## Faste værdier i guiden

Ret dem til, hvis jeg vælger noget andet:

| Hvad | Værdi |
|------|-------|
| Servernavn (hostname) | `homeserver` |
| Brugernavn | `simon` |
| Lokal IP | `192.168.1.50` (eksempel, se [[03 - Første boot og SSH]]) |
| SSH-alias på min PC | `homeserver` |

## Tjekliste

- [ ] BIOS opsat (boot fra USB, power on efter strømsvigt)
- [ ] Ubuntu Server installeret
- [ ] SSH-login med nøgle fra primær PC virker
- [ ] Skærm taget af
- [ ] herdr installeret, `herdr --remote homeserver` virker fra primær PC
- [ ] System opdateret, tidszone sat
- [ ] Login med password over SSH slået fra
- [ ] UFW slået til
- [ ] Automatiske sikkerhedsopdateringer slået til
- [ ] Grundværktøjer + git installeret og konfigureret
- [ ] GitHub SSH-nøgle tilføjet
- [ ] micro installeret og sat som standard-editor
- [ ] Tailscale installeret på server og primær PC
- [ ] Key expiry slået fra for serveren i Tailscale
- [ ] Docker + Compose installeret, `hello-world` virker uden sudo
- [ ] Claude Code installeret og logget ind
- [ ] herdr-integration til Claude Code installeret
- [ ] Vault klonet og skills kopieret til serveren
- [ ] Cron job til ugentlig jobsøgning testet og oprettet
- [ ] Cron job til natlig auto pull af git repos testet og oprettet
- [ ] Laptop kan forbinde (se [[15 - Forbind fra laptop]])
- [ ] Backup sat op
