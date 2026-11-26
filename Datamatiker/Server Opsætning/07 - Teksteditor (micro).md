---
tags: [server, linux, homelab, editor]
---

# 07 – Teksteditor: micro

← [[06 - Grundværktøjer og Git]] | Næste: [[08 - Tailscale]] →

**micro** er en terminal-editor (TUI), der føles som VS Code/Notepad: Ctrl+S gemmer, Ctrl+C/V kopierer og indsætter, Ctrl+F søger, og musen virker. Den har syntax highlighting, flere cursors, faner og plugins.

## 1. Installér

```bash
sudo apt install -y micro xclip
micro --version
```

> Er versionen fra apt for gammel, kan den nyeste hentes direkte:
> ```bash
> cd /tmp && curl https://getmic.ro | bash && sudo mv micro /usr/local/bin/
> ```

## 2. Sæt micro som standard-editor

Så bruges micro af `git commit`, `crontab -e`, `sudoedit`, `visudo` osv.

```bash
cat >> ~/.bashrc <<'EOF'
export EDITOR=micro
export VISUAL=micro
export SUDO_EDITOR=micro
EOF
source ~/.bashrc
```

Systemets standard (bruges fx af `sudo visudo`):

```bash
sudo update-alternatives --install /usr/bin/editor editor "$(command -v micro)" 100
sudo update-alternatives --set editor "$(command -v micro)"
```

## 3. Indstillinger

Åbn micro, tryk **Ctrl+E** og skriv fx `set tabsize 4`. Eller rediger filen direkte:

```bash
micro ~/.config/micro/settings.json
```

```json
{
    "clipboard": "terminal",
    "colorscheme": "monokai",
    "tabsize": 4,
    "tabstospaces": true,
    "softwrap": true,
    "savecursor": true,
    "mkparents": true,
    "diffgutter": true
}
```

> [!tip] Kopier/sæt ind over SSH
> `"clipboard": "terminal"` gør, at Ctrl+C i micro (på serveren) havner i udklipsholderen på **min primære PC**. Det kræver en terminal, der understøtter OSC 52 (fx Kitty, WezTerm, Alacritty, Ghostty, Konsole).

## 4. Genveje (minder om VS Code)

| Genvej | Handling |
|--------|----------|
| `Ctrl+S` | Gem |
| `Ctrl+Q` | Luk (spørger, hvis der er noget, der ikke er gemt) |
| `Ctrl+O` | Åbn fil |
| `Ctrl+C` / `Ctrl+X` / `Ctrl+V` | Kopiér / klip / sæt ind |
| `Ctrl+Z` / `Ctrl+Y` | Fortryd / gentag |
| `Ctrl+A` | Markér alt |
| `Ctrl+F` | Søg (`Ctrl+N`/`Ctrl+P` = næste/forrige) |
| `Ctrl+E` | Kommandolinje (fx `replace`, `set`, `help`) |
| `Ctrl+G` | Hjælp / genvejsoversigt |
| `Ctrl+T` | Ny fane |
| `Alt+,` / `Alt+.` | Forrige / næste fane |
| `Alt+N` | Ny cursor ved næste forekomst (som Ctrl+D i VS Code) |
| `Ctrl+L` | Gå til linje |
| `Ctrl+K` | Klip hele linjen |

Søg og erstat: `Ctrl+E` → `replace "gammel" "ny"` (eller `replaceall`).

## 5. Rediger systemfiler

```bash
sudoedit /etc/ssh/sshd_config.d/01-hardening.conf
# eller
sudo micro /etc/hosts
```

`sudoedit` er lidt sikrere: micro kører som min egen bruger, og filen gemmes med sudo bagefter.

## 6. Plugins (valgfrit)

```bash
micro -plugin install filemanager    # filtræ i siden (Ctrl+E → tree)
micro -plugin list
```

---

## Alternativ: VS Code direkte på serveren (Remote-SSH)

Vil jeg have den rigtige VS Code-oplevelse til større projekter, kan VS Code på **min primære PC** åbne mapper på serveren over SSH:

1. Installér udvidelsen **Remote - SSH** i VS Code.
2. `Ctrl+Shift+P` → **Remote-SSH: Connect to Host** → vælg `homeserver` (bruger `~/.ssh/config` fra [[03 - Første boot og SSH]]).
3. Åbn fx `~/docker`.

Terminal, filer, git og udvidelser kører så på serveren. micro bruger jeg til hurtige rettelser, og VS Code Remote til de større ting.

> Andre TUI-editorer der minder om VS Code: **fresh** og **edit** (Microsofts nye terminal-editor). micro er dog den mest modne og nemmeste at installere.

---

Næste: [[08 - Tailscale]] →
