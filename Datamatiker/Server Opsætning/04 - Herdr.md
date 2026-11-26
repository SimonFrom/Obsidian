---
tags: [server, linux, homelab, herdr, terminal]
---

# 04 – Herdr

← [[03 - Første boot og SSH]] | Næste: [[05 - Grundopsætning og sikkerhed]] →

**herdr** er en terminal-workspace-manager lavet til AI coding agents. Den minder om tmux med workspaces, faner og paneler. Sessionerne kører i en server i baggrunden, så de **bliver ved med at køre, når SSH-forbindelsen lukkes eller ryger**. Den kan også se, om en agent (fx Claude Code) arbejder, venter eller er blokeret.

Jeg har allerede herdr på min primære PC (`herdr-bin` fra AUR), så jeg kan koble mig direkte på serveren med `herdr --remote`.

> [!tip] Hvorfor herdr så tidligt?
> Når herdr er sat op, laver jeg **resten af opsætningen inde i herdr**. Ryger forbindelsen midt i en `apt full-upgrade` eller en Docker-installation, kører den videre, og jeg hopper bare ind igen.

## 1. Installér på serveren

```bash
ssh homeserver
curl -fsSL https://herdr.dev/install.sh | sh
```

Log ud og ind igen, så `~/.local/bin` kommer med i PATH (Ubuntus `~/.profile` tilføjer mappen, når den findes):

```bash
exit
ssh homeserver
herdr --version
```

Findes `herdr` ikke:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

> Alternativ: `herdr machine add homeserver` på min primære PC (trin 3) kan selv installere herdr på serveren i `~/.local/bin`.

## 2. Brug herdr direkte på serveren

```bash
ssh homeserver
herdr                      # starter eller hopper ind i den faste session
```

`Ctrl+b` → `q` forlader herdr (detach), og panelerne kører videre. Næste gang jeg skriver `herdr`, er jeg tilbage, hvor jeg slap.

## 3. Brug herdr fra min primære PC (anbefalet)

Herdr på min PC kan koble sig på serveren over SSH. Den bruger aliaset `homeserver` fra `~/.ssh/config` ([[03 - Første boot og SSH]]):

```bash
herdr --remote homeserver
herdr --remote homeserver --session setup     # en navngivet session
```

Gem serveren som en fast maskine, så den også står i herdr's oversigt:

```bash
herdr machine add homeserver --label "Homeserver"
herdr machine list
herdr machine status
```

- Mine **lokale keybindings** bruges som standard. `--remote-keybindings server` bruger serverens config i stedet.
- Har min SSH-nøgle en passphrase, kører jeg `ssh-add` først.
- Virker det ikke, tester jeg først almindelig `ssh homeserver`.

## 4. Vigtigste genveje

Prefix er **`Ctrl+b`** (som i tmux). Tryk prefix, slip, og tryk så tasten.

| Genvej | Handling |
|--------|----------|
| `Ctrl+b` `c` | Ny fane |
| `Ctrl+b` `v` | Del til højre |
| `Ctrl+b` `-` | Del nedad |
| `Ctrl+b` `h/j/k/l` | Skift panel |
| `Ctrl+b` `n` / `p` | Næste / forrige fane |
| `Ctrl+b` `1`–`9` | Hop til fane |
| `Ctrl+b` `Shift+n` | Nyt workspace |
| `Ctrl+b` `w` | Skift workspace |
| `Ctrl+b` `z` | Zoom panel (fuld skærm) |
| `Ctrl+b` `x` | Luk panel |
| `Ctrl+b` `r` | Resize-tilstand |
| `Ctrl+b` `[` | Copy/scroll-tilstand |
| `Ctrl+b` `g` | Goto-vælger |
| `Ctrl+b` `b` | Vis/skjul sidebar |
| `Ctrl+b` `q` | Detach (panelerne kører videre) |
| `Ctrl+b` `?` | Vis alle genveje |

Musen virker til alt: klik, træk i kanterne og markér for at kopiere.

> [!warning] Ikke tmux inde i herdr
> Begge bruger `Ctrl+b`. Jeg bruger herdr til sessioner og starter ikke tmux inde i herdr. tmux ([[06 - Grundværktøjer og Git#4. tmux – det vigtigste at vide]]) er kun en reserve, hvis herdr driller.

## 5. Sessioner

```bash
herdr session list
herdr session attach setup
herdr session stop setup
herdr server stop          # stopper hele herdr-serveren og alle paneler
```

## 6. Config (valgfrit)

Configfilen ligger i `~/.config/herdr/config.toml`. Samme tema som på min PC:

```toml
onboarding = false

[theme]
name = "gruvbox"
```

Efter en ændring: `herdr server reload-config`.

Eller kopiér hele min config fra den primære PC:

```bash
ssh homeserver 'mkdir -p ~/.config/herdr'
scp ~/.config/herdr/config.toml homeserver:~/.config/herdr/
```

## 7. Claude Code-integration (senere)

Når Claude Code er installeret ([[10 - Claude Code]]), kobles herdr på, så den kan se Claude's status:

```bash
herdr integration install claude
herdr integration status
```

## 8. Opdatering

```bash
herdr update
```

---

Næste: [[05 - Grundopsætning og sikkerhed]] → (kør det inde i herdr)
