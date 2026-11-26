---
tags: [server, linux, homelab, ssh, tailscale, herdr]
---

# 15 – Forbind fra min laptop

← [[00 - Oversigt]]

Sådan sætter jeg min laptop op, så den kan forbinde til serveren på samme måde som min primære PC: med SSH, Tailscale og herdr, både hjemme og ude.

Laptoppen kører samme opsætning som desktoppen (**CachyOS + Noctalia shell**), så kommandoerne er de samme, og config-filerne kan kopieres direkte fra desktoppen.

> [!info] Forudsætninger
> Serveren er sat op til og med [[08 - Tailscale]]. Det her kan laves når som helst bagefter.

> [!warning] Password-login er slået fra
> I [[05 - Grundopsætning og sikkerhed]] slog jeg login med password fra. `ssh-copy-id` fra laptoppen virker derfor **ikke**. Laptoppens nøgle skal lægges på serveren på en anden måde, se trin 3.

## 1. Installér værktøjerne på laptoppen

```bash
sudo pacman -S --needed openssh tailscale
paru -S herdr-bin                      # samme pakke som på desktoppen
sudo systemctl enable --now tailscaled
```

> Tailscale styres fra terminalen (`tailscale up/status`). Der skal ikke installeres en tray-app for at det virker.

## 2. Lav en SSH-nøgle til laptoppen

Hver enhed skal have sin **egen** nøgle. Jeg kopierer ikke den private nøgle fra min primære PC. Så kan jeg spærre laptoppen alene, hvis den bliver væk.

```bash
ssh-keygen -t ed25519 -C "laptop"
cat ~/.ssh/id_ed25519.pub
```

## 3. Læg laptoppens nøgle på serveren

Jeg vælger én af metoderne:

### Mulighed A – via GitHub (nemmest)

1. Tilføj laptoppens offentlige nøgle på GitHub: **Settings** → **SSH and GPG keys** → **New SSH key**, titel `laptop`.
2. Kør på serveren (fra min primære PC: `ssh homeserver`):

```bash
ssh-import-id gh:<mit-github-brugernavn>
```

Kommandoen henter alle mine offentlige nøgler fra GitHub og tilføjer de nye til `~/.ssh/authorized_keys`. Nøgler, der allerede er der, bliver ikke tilføjet igen.

### Mulighed B – via min primære PC

Få `id_ed25519.pub` fra laptoppen over på min primære PC (det er en offentlig nøgle, så den må gerne sendes). Fx med Taildrop, når Tailscale kører på begge:

```bash
# på laptoppen:
tailscale file cp ~/.ssh/id_ed25519.pub <navn-på-primær-pc>:
# på min primære PC:
sudo tailscale file get ~/Downloads/
```

Tilføj den så til serveren fra min **primære PC**:

```bash
ssh homeserver 'cat >> ~/.ssh/authorized_keys' < ~/Downloads/id_ed25519.pub
```

### Mulighed C – Tailscale SSH

Kører serveren med `tailscale up --ssh` ([[08 - Tailscale#7. Valgfrit: Tailscale SSH]]), styrer Tailscale adgangen, og der skal ingen nøgle på serveren. Det kræver, at laptoppen er logget ind på den samme Tailscale-konto.

> [!danger] Slå ikke password-login til igen for at få det til at virke
> Det er hurtigt, men det åbner op for brute force. A eller B tager to minutter.

## 4. Tailscale på laptoppen

```bash
sudo tailscale up
```

Log ind med **samme konto** som serveren. Tjek:

```bash
tailscale status             # homeserver skal stå på listen
ping homeserver
```

For laptoppen lader jeg **key expiry** være slået til. Den skal kun slås fra på serveren.

## 5. SSH-config på laptoppen

Det nemmeste er at kopiere `~/.ssh/config` fra desktoppen. Der står `IdentityFile ~/.ssh/id_ed25519`, og den sti peger på laptoppens egen nøgle fra trin 2. Kør på **laptoppen**:

```bash
scp <desktop-navn>:~/.ssh/config ~/.ssh/config     # via Tailscale, kræver SSH på desktoppen
# eller kopiér filen med USB / Taildrop
chmod 600 ~/.ssh/config
```

Ellers laver jeg den selv:

```ssh-config
Host homeserver
    HostName homeserver          # Tailscale MagicDNS – virker hjemme og ude
    User simon
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60

Host homeserver-lan
    HostName 192.168.1.50        # kun hjemme, hvis Tailscale er nede
    User simon
    IdentityFile ~/.ssh/id_ed25519
```

Test:

```bash
ssh homeserver
```

## 6. herdr fra laptoppen

```bash
herdr --remote homeserver
herdr machine add homeserver --label "Homeserver"
```

Sessionerne kører på serveren, så jeg kan **fortsætte arbejdet fra min primære PC på laptoppen**. Har jeg brugt en navngivet session:

```bash
herdr --remote homeserver --session setup
```

En Claude Code-session, der kører i herdr på serveren, kan jeg altså starte på min primære PC og følge med i fra laptoppen.

Kopiér min herdr-config fra desktoppen, så laptoppen bruger samme tema (gruvbox) og keybindings:

```bash
mkdir -p ~/.config/herdr
scp homeserver:~/.config/herdr/config.toml ~/.config/herdr/   # kopien fra trin 04 på serveren
```

## 7. VS Code Remote-SSH (valgfrit)

Samme fremgangsmåde som på min primære PC ([[07 - Teksteditor (micro)#Alternativ: VS Code direkte på serveren (Remote-SSH)]]): installér **Remote - SSH**, og forbind til `homeserver`.

## 8. Hvis laptoppen bliver væk

1. **Tailscale admin console** → Machines → laptoppen → **Remove**.
2. Fjern nøglen på serveren:

```bash
micro ~/.ssh/authorized_keys      # slet linjen der ender på "laptop"
```

3. Slet nøglen på GitHub (Settings → SSH and GPG keys), hvis jeg brugte mulighed A.

## Tjekliste

- [ ] Tailscale, SSH og herdr installeret på laptoppen
- [ ] Ny SSH-nøgle lavet på laptoppen
- [ ] Nøglen lagt på serveren (GitHub, primær PC eller Tailscale SSH)
- [ ] Laptoppen logget ind på Tailscale
- [ ] `~/.ssh/config` sat op, og `ssh homeserver` virker
- [ ] `herdr --remote homeserver` virker
