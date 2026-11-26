---
tags: [server, linux, homelab, claude]
---

# 10 – Claude Code

← [[09 - Docker]] | Næste: [[11 - Cron job - Jobsøgning]] →

## 1. Installér (native installer)

Den officielle installer kræver ikke Node.js og opdaterer sig selv automatisk:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Installeren lægger `claude` i `~/.local/bin`. Tjek, at den mappe er i PATH:

```bash
source ~/.bashrc     # eller log ud og ind igen
which claude
claude --version
```

Findes `claude` ikke:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## 2. Log ind (uden skærm/browser på serveren)

Start Claude Code **i herdr**, så det bliver ved med at køre, selvom SSH ryger:

```bash
# på min primære PC:
herdr --remote homeserver
# i et nyt panel/fane (Ctrl+b c):
claude
```

1. Vælg tema og login-metode (Claude-abonnement eller API-konto).
2. Claude Code viser et **link**. Serveren har ingen browser, så kopiér linket og åbn det på **min primære PC**.
3. Log ind og godkend. Kopiér den kode, browseren viser, og sæt den ind i terminalen på serveren.

Tjek installationen:

```bash
claude doctor
```

### Kobl herdr på Claude Code

Så kan herdr vise, om Claude arbejder, venter på mig eller er blokeret (se [[04 - Herdr#7. Claude Code-integration (senere)]]):

```bash
herdr integration install claude
herdr integration status
```

## 3. Giv Claude kontekst om serveren

Lav en `CLAUDE.md` i den mappe, jeg normalt starter Claude fra (fx `~/docker`), så Claude ved, hvordan serveren er sat op:

```bash
micro ~/docker/CLAUDE.md
```

```markdown
# Homeserver

- Ubuntu Server LTS på en mini PC, hostname `homeserver`
- Adgang via SSH og Tailscale (MagicDNS-navn: homeserver)
- Hver tjeneste ligger i sin egen mappe her i ~/docker med en compose.yaml
- Hemmeligheder ligger i .env-filer og må aldrig committes
- Firewall: UFW. Docker-porte går uden om UFW – bind til 127.0.0.1 eller Tailscale-IP når en tjeneste ikke skal være på LAN
- Editor: micro
- Spørg før du kører kommandoer med sudo, sletter volumes eller genstarter serveren
```

## 4. Gode måder at bruge den på

```bash
cd ~/docker
claude
```

Eksempler på opgaver:

- *"Lav en compose.yaml til Jellyfin med mine film i /srv/media"*
- *"Hvorfor genstarter containeren uptime-kuma hele tiden? Kig i logs"*
- *"Hvad fylder mest på disken?"*
- *"Opdatér alle mine compose-tjenester til nyeste images"*

> [!warning] Rettigheder
> Claude Code spørger, før den kører kommandoer eller ændrer filer. På en server skal jeg især læse `sudo`-kommandoer, `rm` og `docker volume rm`, før jeg godkender dem. Godkend ikke automatisk alt.

## 5. Tilbage i sessionen

```bash
herdr                      # på serveren efter ssh homeserver, eller fra min primære PC: herdr --remote homeserver
```

Inde i herdr: `Ctrl+b q` forlader sessionen uden at stoppe Claude.

## 6. Opdatering

Native installer opdaterer automatisk. Manuelt:

```bash
claude update
```

---

Næste: [[11 - Cron job - Jobsøgning]] →
