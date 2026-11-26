---
tags: [server, linux, homelab, claude, cron, job]
---

# 12 – Cron job: ugentlig jobsøgning

← [[09 - Claude Code]] | Næste: [[13 - Cron job - Auto pull af git repos]] →

Serveren kører min `job-soegning`-skill med Claude Code **én gang om ugen om natten** (natten til mandag kl. 03:00). Skill'en læser søgefilen i vaulten, skriver nye opslag i `Skal søges.md` og laver udkast til ansøgninger på Google Drev. Når den er færdig, bliver ændringerne pushet til GitHub, så de dukker op i Obsidian på min primære PC.

```
cron (man. 03:00) → job-soegning.sh → git pull → claude -p "/job-soegning" → git commit + push
```

> [!info] Forudsætninger
> - [[05 - Grundværktøjer og Git]]: serverens SSH-nøgle er lagt på GitHub
> - [[09 - Claude Code]]: Claude Code er installeret og logget ind med min **claude.ai-konto**. Google Drev-connectoren følger med kontoen, og skill'en har brug for den.

## 1. Hent vaulten til serveren

Skill'en forventer, at vaulten ligger i `/home/simon/Documents/Obsidian/Obsidian`. Derfor bruger jeg den samme sti på serveren:

```bash
mkdir -p ~/Documents/Obsidian
git clone git@github.com:SimonFrom/Obsidian.git ~/Documents/Obsidian/Obsidian
cd ~/Documents/Obsidian/Obsidian
git config user.name "homeserver"
git config user.email "min@email.dk"
```

## 2. Kopiér skills til serveren

Skill'erne ligger på min **primære PC** i `~/.claude/skills/`. `job-soegning` bruger `job-ansoegning` til udkastene, så begge skal med. Kør på min **primære PC**:

```bash
ssh homeserver 'mkdir -p ~/.claude/skills'
scp -r ~/.claude/skills/job-soegning ~/.claude/skills/job-ansoegning homeserver:~/.claude/skills/
```

> Når jeg retter en skill på min PC, skal kommandoen køres igen. Ellers bruger serveren den gamle udgave.

## 3. Test manuelt først

Første kørsel laver jeg **interaktivt**. Så kan jeg godkende Google Drev-adgang og se, hvilke værktøjer skill'en beder om:

```bash
cd ~/Documents/Obsidian/Obsidian
claude
```

```
/job-soegning
```

Tjek bagefter `Datamatiker/Job/Jobsøgning/Kørselslog.md` og Google Drev.

## 4. Script, der står for kørslen

```bash
mkdir -p ~/bin ~/logs
micro ~/bin/job-soegning.sh
```

```bash
#!/usr/bin/env bash
# Ugentlig jobsøgning med Claude Code. Køres af cron.
set -euo pipefail

VAULT="$HOME/Documents/Obsidian/Obsidian"
LOG="$HOME/logs/job-soegning-$(date +%F).log"

# cron har en meget kort PATH – claude ligger i ~/.local/bin
export PATH="$HOME/.local/bin:/usr/local/bin:/usr/bin:/bin"

# Værktøjer, Claude må bruge uden at spørge (der er ingen til at godkende om natten)
ALLOWED_TOOLS="Read,Write,Edit,Glob,Grep,WebFetch,WebSearch,Agent,Skill,Bash(mv:*),Bash(ls:*),Bash(mkdir:*),mcp__claude_ai_Google_Drive,mcp__claude_ai_Google_Docs"

{
  echo "=== Start $(date '+%F %T') ==="
  cd "$VAULT"

  # Hent de seneste ændringer fra min primære PC
  git pull --rebase --autostash

  # Kør skill'en uden interaktion
  claude -p "/job-soegning" \
    --permission-mode acceptEdits \
    --allowedTools "$ALLOWED_TOOLS"

  # Gem og send ændringerne tilbage til GitHub
  git add -A
  if git diff --cached --quiet; then
    echo "Ingen ændringer at committe"
  else
    git commit -m "jobsøgning: automatisk kørsel $(date +%F)"
    git push
  fi

  echo "=== Slut $(date '+%F %T') ==="
} >> "$LOG" 2>&1

# Slet logs ældre end 90 dage
find "$HOME/logs" -name 'job-soegning-*.log' -mtime +90 -delete
```

```bash
chmod +x ~/bin/job-soegning.sh
```

> [!note] Hvorfor commit i scriptet?
> Skill'en må selv **ikke** committe i vault-repoet. Det er scriptet, der gør det bagefter, så Claude aldrig rører git.

> [!warning] SSH-nøgle med passphrase
> cron har ingen ssh-agent. Har serverens GitHub-nøgle en passphrase, fejler `git pull/push` om natten. Lav så en separat nøgle uden passphrase kun til det her (eller brug en *deploy key* med skriveadgang til Obsidian-repoet).

## 5. Test scriptet, som cron ville køre det

cron kører med et næsten tomt miljø. Test med samme betingelser:

```bash
env -i HOME="$HOME" USER="$USER" /bin/bash -c '$HOME/bin/job-soegning.sh'
cat ~/logs/job-soegning-$(date +%F).log
```

Står der i loggen, at et værktøj blev afvist (*permission denied* / *not allowed*), tilføjer jeg det til `ALLOWED_TOOLS` og tester igen.

## 6. Opret cron job

```bash
crontab -e
```

Tilføj nederst:

```cron
# Jobsøgning – natten til mandag kl. 03:00
0 3 * * 1 /usr/bin/flock -n /tmp/job-soegning.lock $HOME/bin/job-soegning.sh
```

| Felt | Værdi | Betyder |
|------|-------|---------|
| minut | `0` | :00 |
| time | `3` | kl. 03 |
| dag i måneden | `*` | alle |
| måned | `*` | alle |
| ugedag | `1` | mandag (0/7 = søndag) |

- Tiden følger serverens tidszone. Den blev sat til `Europe/Copenhagen` i [[04 - Grundopsætning og sikkerhed]].
- `flock -n` sørger for, at to kørsler aldrig kører samtidig.
- Andet tidspunkt? Brug <https://crontab.guru> til at lave udtrykket.

Tjek at den er gemt:

```bash
crontab -l
```

## 7. På min primære PC

Obsidian Git-pluginet skal **hente** ændringerne fra serveren. Under Obsidian Git-indstillingerne slår jeg til:

- **Pull on startup**
- Gerne også **Auto pull interval** (fx 30 min)

Så står mandagens nye jobopslag klar i `Skal søges.md`, når jeg åbner Obsidian.

## 8. Tjek at det virker

Efter første mandag:

```bash
ls ~/logs/
tail -50 ~/logs/job-soegning-*.log
grep CRON /var/log/syslog | tail      # kørte cron overhovedet?
```

og i vaulten: `Datamatiker/Job/Jobsøgning/Kørselslog.md`.

## Fejlfinding

| Problem | Løsning |
|---------|---------|
| Ingen logfil | cron kørte ikke: tjek `crontab -l` og `grep CRON /var/log/syslog` |
| `claude: command not found` | PATH i scriptet passer ikke: tjek `which claude` |
| `git pull` fejler med konflikt | Jeg har rettet de samme linjer på min PC. Løs det manuelt i `~/Documents/Obsidian/Obsidian` (`git status`) |
| `Permission denied (publickey)` | SSH-nøglen har en passphrase eller ligger ikke på GitHub, se trin 4 |
| Ingen udkast på Drevet | Google Drev-connectoren er ikke godkendt: kør `claude` interaktivt og test, se trin 3 |
| Login udløbet | Kør `claude` interaktivt, og log ind igen |

> [!tip] Alternativ: systemd-timer
> Skal kørslen tages igen, hvis serveren var slukket kl. 03, kan en systemd-timer med `Persistent=true` bruges i stedet for cron. En server, der altid er tændt, kan sagtens bruge cron.

---

Næste: [[13 - Cron job - Auto pull af git repos]] →
