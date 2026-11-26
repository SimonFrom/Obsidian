---
tags: [server, linux, homelab, netværk, tailscale]
---

# 08 – Tailscale

← [[07 - Teksteditor (micro)]] | Næste: [[09 - Docker]] →

Tailscale laver et privat netværk (VPN, bygget på WireGuard) mellem mine enheder. Så kan jeg nå serveren fra min primære PC, laptop og telefon, også uden for hjemmet, **uden at åbne porte i routeren**.

## 1. Opret konto

Gå til <https://login.tailscale.com> og log ind med GitHub/Google. Den gratis *Personal*-plan er rigelig.

## 2. Installér på serveren

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

`tailscale up` viser et link. Åbn det på min primære PC, og godkend serveren.

Tjek:

```bash
tailscale status
tailscale ip -4      # serverens Tailscale-IP, fx 100.x.y.z
```

## 3. Slå key expiry fra for serveren

Som standard skal enheder logge ind igen efter 180 dage. På en server uden skærm er det irriterende, for så mister jeg adgangen.

Tailscale admin console → **Machines** → `homeserver` → **⋯** → **Disable key expiry**.

## 4. Installér på min primære PC (CachyOS/Arch)

```bash
sudo pacman -S tailscale
sudo systemctl enable --now tailscaled
sudo tailscale up
```

Installér også appen på telefonen, hvis jeg vil kunne nå serveren derfra. Laptoppen: se [[15 - Forbind fra laptop]].

## 5. MagicDNS – brug navnet i stedet for IP

MagicDNS er slået til som standard. Fra min primære PC:

```bash
ping homeserver
ssh simon@homeserver
```

Opdatér `~/.ssh/config` på min primære PC, så genvejen virker overalt:

```ssh-config
Host homeserver
    HostName homeserver          # Tailscale MagicDNS-navn (virker hjemme og ude)
    User simon
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60

# Bruges hvis Tailscale er nede:
Host homeserver-lan
    HostName 192.168.1.50
    User simon
    IdentityFile ~/.ssh/id_ed25519
```

## 6. Firewall: stol på Tailscale-netværket

Tillad al trafik, der kommer ind via Tailscale. Så kan jeg nå tjenester på serveren fra mine egne enheder uden at åbne dem for hele det lokale netværk:

```bash
sudo ufw allow in on tailscale0
sudo ufw status
```

## 7. Valgfrit: Tailscale SSH

Tailscale kan også stå for selve SSH-login (adgangen styres af Tailscale-kontoen, så der ikke skal styres nøgler):

```bash
sudo tailscale up --ssh
```

Normal SSH med nøgler virker også fint, så det er ikke nødvendigt.

## 8. Valgfrit: Exit node / subnet router

- **Exit node** – brug hjemmets internetforbindelse, når jeg er på offentligt WiFi:
  ```bash
  echo 'net.ipv4.ip_forward = 1' | sudo tee /etc/sysctl.d/99-tailscale.conf
  echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
  sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
  sudo tailscale set --advertise-exit-node
  ```
  Godkend derefter i admin console → Machines → `homeserver` → *Edit route settings*.
- **Subnet router** – giv adgang til hele hjemmenetværket (fx printer, NAS): `--advertise-routes=192.168.1.0/24`.

---

Næste: [[09 - Docker]] →
