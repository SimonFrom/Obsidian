---
tags: [server, linux, homelab, git, cron]
---

# 13 – Cron job: auto pull af alle git repos

← [[12 - Cron job - Jobsøgning]] | Næste: [[10 - Næste skridt]] →

Hver nat kl. 02:00 finder serveren alle git repos i min hjemmemappe og kører `git pull` i dem. Så har serveren altid de nyeste ændringer fra GitHub, fx dem jeg har pushet fra min primære PC.

> [!info] Det sikre valg: `--ff-only`
> Scriptet ændrer aldrig noget, jeg selv har lavet på serveren:
> - Har et repo **ucommittede ændringer**, springes det over.
> - Har et repo **ingen upstream** (ingen remote branch), springes det over.
> - Pull laves med `--ff-only`, så der aldrig bliver lavet merge-commits eller konflikter. Er branchen gået forskellige veje, skrives det i loggen, og jeg ordner det selv.

> [!info] Forudsætninger
> - [[05 - Grundværktøjer og Git]]: serverens SSH-nøgle er lagt på GitHub og har **ingen passphrase** (cron har ingen ssh-agent, se [[12 - Cron job - Jobsøgning#4. Script, der står for kørslen]])

## 1. Scriptet

```bash
mkdir -p ~/bin ~/logs
micro ~/bin/git-autopull.sh
```

```bash
#!/usr/bin/env bash
# Natlig git pull af alle repos i hjemmemappen. Køres af cron.
set -uo pipefail   # ikke -e: én fejl må ikke stoppe resten af repoerne

export PATH="$HOME/.local/bin:/usr/local/bin:/usr/bin:/bin"

# Mapper der søges i, og hvor dybt
ROOTS=("$HOME")
MAXDEPTH=4

LOG="$HOME/logs/git-autopull-$(date +%F).log"
mkdir -p "$HOME/logs"

{
  echo "=== Start $(date '+%F %T') ==="

  find "${ROOTS[@]}" -maxdepth "$MAXDEPTH" \
       \( -name node_modules -o -name .cache -o -name .local -o -name .npm \) -prune \
       -o -type d -name .git -print0 2>/dev/null |
  while IFS= read -r -d '' gitdir; do
    repo="$(dirname "$gitdir")"
    cd "$repo" || continue

    # Ingen upstream → spring over
    if ! git rev-parse --abbrev-ref '@{u}' >/dev/null 2>&1; then
      echo "SKIP (ingen upstream):        $repo"
      continue
    fi

    # Lokale ændringer → spring over (ignorerer untracked filer)
    if [ -n "$(git status --porcelain --untracked-files=no)" ]; then
      echo "SKIP (ucommittede ændringer): $repo"
      continue
    fi

    if out=$(timeout 120 git pull --ff-only 2>&1); then
      echo "OK:   $repo — $(echo "$out" | tail -n1)"
    else
      echo "FEJL: $repo"
      echo "$out" | sed 's/^/      /'
    fi
  done

  echo "=== Slut $(date '+%F %T') ==="
} >> "$LOG" 2>&1

# Slet logs ældre end 30 dage
find "$HOME/logs" -name 'git-autopull-*.log' -mtime +30 -delete
```

```bash
chmod +x ~/bin/git-autopull.sh
```

**Andre mapper eller flere niveauer?** Ret `ROOTS`, fx `ROOTS=("$HOME/docker" "$HOME/projekter" "$HOME/Documents")`, og ret `MAXDEPTH`, hvis repoerne ligger dybere.

## 2. Test

```bash
env -i HOME="$HOME" USER="$USER" /bin/bash -c '$HOME/bin/git-autopull.sh'
cat ~/logs/git-autopull-$(date +%F).log
```

Eksempel på output:

```
=== Start 2026-11-27 02:00:01 ===
OK:   /home/simon/docker — Already up to date.
OK:   /home/simon/Documents/Obsidian/Obsidian — Fast-forward
SKIP (ucommittede ændringer): /home/simon/projekter/test
=== Slut 2026-11-27 02:00:09 ===
```

## 3. Opret cron job

```bash
crontab -e
```

Tilføj under jobsøgningen:

```cron
# Auto pull af alle git repos – hver nat kl. 02:00
0 2 * * * /usr/bin/flock -n /tmp/git-autopull.lock $HOME/bin/git-autopull.sh
```

- Kl. 02:00 ligger før jobsøgningen kl. 03:00 om mandagen, så de ikke kører samtidig på vaulten.
- Tjek med `crontab -l`.

## 4. Docker-tjenester bliver ikke genstartet

Et `git pull` i `~/docker` henter de nye `compose.yaml`-filer, men containerne kører videre med den gamle opsætning. Ændringerne bliver først brugt, når jeg selv kører:

```bash
cd ~/docker/<tjeneste> && docker compose up -d
```

Det er med vilje. Så bliver en tjeneste aldrig genstartet om natten, uden at jeg ved det.

## Fejlfinding

| I loggen | Betyder / løsning |
|----------|-------------------|
| `FEJL ... Not possible to fast-forward` | Der ligger lokale commits på serveren, som ikke er pushet. Gå ind i repoet, og kør `git pull --rebase` eller `git push` manuelt |
| `FEJL ... Permission denied (publickey)` | SSH-nøglen har en passphrase eller er ikke på GitHub |
| `SKIP (ucommittede ændringer)` | Jeg har rettet filer på serveren. Commit, `git stash` eller `git restore` dem |
| Et repo mangler helt i loggen | Det ligger dybere end `MAXDEPTH` eller uden for `ROOTS` |
| Ingen logfil | cron kørte ikke: `grep CRON /var/log/syslog \| tail` |
