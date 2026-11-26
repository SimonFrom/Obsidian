---
tags: [server, linux, homelab, ssh]
---

# 03 – Første boot og SSH

← [[02 - Installer Ubuntu Server]] | Næste: [[04 - Herdr]] →

Målet med det her trin er at kunne logge ind fra min primære PC og så tage skærmen af.

## 1. Log ind på serveren (med skærm)

Efter genstart står der `homeserver login:`. Log ind med `simon` og mit password.

> Der kan stå nogle *cloud-init*-beskeder hen over login-prompten under første opstart. Tryk **Enter**, så kommer prompten frem igen.

Find serverens IP-adresse:

```bash
hostname -I
# eller med flere detaljer:
ip -br a
```

Det er den adresse, der starter med `192.168.` (eller `10.`), fx `192.168.1.50`.

Tjek, at SSH kører:

```bash
systemctl status ssh
```

> I nyere Ubuntu-udgaver bliver SSH startet via `ssh.socket`. Står der *inactive* for `ssh.service`, men `systemctl status ssh.socket` siger *active (listening)*, så virker det alligevel.

## 2. Giv serveren en fast IP (DHCP-reservation i routeren)

1. Log ind på routerens admin-side.
2. Find **DHCP** / **LAN** / *Address reservation* / *Static lease*.
3. Find `homeserver` på listen over enheder, og reservér dens nuværende IP.

Så får serveren altid den samme IP, uden at jeg skal ændre noget på serveren.

> [!note] Alternativ: fast IP på selve serveren (netplan)
> Kan routeren ikke lave reservationer, kan IP'en sættes i `/etc/netplan/`. Det er beskrevet i [[14 - Fejlfinding#Fast IP med netplan]].

## 3. Første SSH-login fra min primære PC

På **min primære PC**:

```bash
ssh simon@192.168.1.50
```

Første gang spørger den, om jeg stoler på serverens fingerprint → skriv `yes`.

Virker det, er jeg inde. 🎉

## 4. SSH-nøgle (login uden password)

### 4a. Har jeg en nøgle på min primære PC?

```bash
ls ~/.ssh/id_ed25519.pub
```

Hvis ikke, laver jeg en:

```bash
ssh-keygen -t ed25519 -C "primaer-pc"
# Tryk Enter for standard-placering. Brug gerne en passphrase.
```

### 4b. Kopiér nøglen til serveren

Hvis jeg ikke importerede nøglen fra GitHub under installationen:

```bash
ssh-copy-id simon@192.168.1.50
```

Test, om den virker – jeg må **ikke** blive spurgt om serverens password (måske om nøglens passphrase, og det er fint):

```bash
ssh simon@192.168.1.50
```

## 5. Genvej i SSH-config

På **min primære PC**, rediger `~/.ssh/config`:

```bash
micro ~/.ssh/config    # eller en anden editor
```

```ssh-config
Host homeserver
    HostName 192.168.1.50
    User simon
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
```

Nu er det nok at skrive:

```bash
ssh homeserver
```

> Når Tailscale er sat op ([[08 - Tailscale]]), ændrer jeg `HostName` til serverens Tailscale-navn, så genvejen også virker uden for hjemmet.

## 6. Tag skærmen af 🖥️❌

Når `ssh homeserver` virker med nøgle, kan skærm og tastatur fjernes. Resten af guiden foregår over SSH, og fra næste trin inde i herdr.

> [!warning] Inden skærmen tages af
> Tjek at jeg kan logge ind med **nøgle**, og at jeg kan køre `sudo` (fx `sudo whoami` → `root`). Ellers kan jeg risikere at skulle sætte skærmen på igen.

---

Næste: [[04 - Herdr]] →
