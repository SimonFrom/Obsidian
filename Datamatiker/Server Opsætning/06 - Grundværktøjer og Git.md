---
tags: [server, linux, homelab, git]
---

# 06 – Grundværktøjer og Git

← [[05 - Grundopsætning og sikkerhed]] | Næste: [[07 - Teksteditor (micro)]] →

## 1. Installér pakkerne

```bash
sudo apt install -y \
  git curl wget ca-certificates gnupg \
  tmux btop htop ncdu tree \
  unzip zip jq \
  build-essential \
  net-tools dnsutils \
  bash-completion
```

| Pakke | Hvad bruges den til |
|-------|---------------------|
| `git` | Versionsstyring, hente repos |
| `curl`, `wget` | Hente filer/scripts fra nettet (bruges i Tailscale, Docker og Claude Code) |
| `tmux` | Reserve til [[04 - Herdr]], hvis herdr driller |
| `btop` / `htop` | Se CPU, RAM, diske, processer |
| `ncdu` | Find ud af, hvad der fylder på disken |
| `jq` | Læse/formatere JSON i terminalen |
| `build-essential` | Compiler m.m. – nogle værktøjer kræver det |
| `dnsutils` | `dig` til DNS-fejlfinding |

## 2. Konfigurér Git

```bash
git config --global user.name "Simon"
git config --global user.email "min@email.dk"     # samme som på GitHub
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global core.editor micro             # micro installeres i næste trin
```

Tjek:

```bash
git config --global --list
```

## 3. SSH-nøgle til GitHub (fra serveren)

Serveren skal have sin **egen** nøgle. Jeg kopierer ikke min primære PC's private nøgle derover.

```bash
ssh-keygen -t ed25519 -C "homeserver"
cat ~/.ssh/id_ed25519.pub
```

1. Kopiér outputtet.
2. GitHub → **Settings** → **SSH and GPG keys** → **New SSH key**. Titel: `homeserver`.
3. Test:

```bash
ssh -T git@github.com
# "Hi <brugernavn>! You've successfully authenticated..."
```

### GitHub CLI (valgfrit)

```bash
sudo apt install -y gh
gh auth login      # vælg GitHub.com → SSH → "Login with a web browser"
```

Den viser en kode og et link. Åbn linket på min primære PC, og skriv koden.

## 4. tmux – det vigtigste at vide

Til daglig bruger jeg [[04 - Herdr]]. tmux er en reserve, der altid er der (fx hvis herdr er nede efter en opdatering). Jeg starter **ikke** tmux inde i herdr, fordi begge bruger `Ctrl+b`.

```bash
tmux new -s main        # ny session med navnet "main"
# ... arbejd ...
# Ctrl+b, derefter d     → "detach" (sessionen kører videre)
tmux ls                 # vis sessioner
tmux attach -t main     # hop ind i sessionen igen
```

| Genvej (tryk Ctrl+b først) | Handling |
|----------------------------|----------|
| `d` | Gå ud af sessionen (den kører videre) |
| `c` | Nyt vindue |
| `n` / `p` | Næste / forrige vindue |
| `%` | Del lodret |
| `"` | Del vandret |
| piletaster | Skift mellem paneler |
| `[` | Scroll-tilstand (q for at stoppe) |

Lille config med musestøtte (scroll og klik):

```bash
cat >> ~/.tmux.conf <<'EOF'
set -g mouse on
set -g history-limit 50000
set -g base-index 1
EOF
```

## 5. Små forbedringer til shellen (valgfrit)

Tilføj i bunden af `~/.bashrc`:

```bash
alias ll='ls -alh'
alias update='sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y'
alias dc='docker compose'
alias dps='docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"'
```

```bash
source ~/.bashrc
```

> Kan jeg bedre lide fish (som på min primære PC)? `sudo apt install fish && chsh -s /usr/bin/fish`. Bemærk, at mange guides skriver bash-syntaks, så for en server er bash ofte det nemmeste.

---

Næste: [[07 - Teksteditor (micro)]] →
