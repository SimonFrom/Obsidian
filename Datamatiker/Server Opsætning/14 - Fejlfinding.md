---
tags: [server, linux, homelab, fejlfinding]
---

# 14 – Fejlfinding

← [[00 - Oversigt]]

## SSH

**`Connection refused` / `timed out`**
- Er serveren tændt og på netværket? `ping 192.168.0.171`
- Har IP'en ændret sig? Sæt skærmen på, og kør `hostname -I`. Lav en DHCP-reservation ([[03 - Første boot og SSH]]).
- Kører SSH? `systemctl status ssh ssh.socket`
- Blokerer UFW? `sudo ufw status` → `OpenSSH` skal stå som *ALLOW*.

**`Permission denied (publickey)`**
- Ligger min nøgle på serveren? `cat ~/.ssh/authorized_keys` (på serveren).
- Rettigheder på serveren:
  ```bash
  chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
  ```
- Se, hvad klienten prøver: `ssh -v homeserver`

**`WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED`**
Det sker typisk efter en geninstallation af serveren. På min primære PC:
```bash
ssh-keygen -R 192.168.0.171
ssh-keygen -R homeserver
```

**Låst ude efter SSH-hærdning**
Sæt skærm og tastatur på, log ind lokalt, og ret/slet `/etc/ssh/sshd_config.d/01-hardening.conf`. Kør derefter `sudo systemctl restart ssh`.

## Netværk

### Fast IP med netplan

Kun hvis routeren ikke kan lave DHCP-reservation. Find navnet på netværkskortet med `ip -br a` (fx `enp1s0`):

```bash
ls /etc/netplan/
sudo micro /etc/netplan/50-cloud-init.yaml
```

```yaml
network:
  version: 2
  ethernets:
    enp1s0:
      dhcp4: false
      addresses: [192.168.0.171/24]
      routes:
        - to: default
          via: 192.168.0.1
      nameservers:
        addresses: [1.1.1.1, 9.9.9.9]
```

```bash
sudo chmod 600 /etc/netplan/*.yaml
sudo netplan try      # ruller automatisk tilbage efter 120 sek, hvis jeg ikke bekræfter
```

Brug `netplan try` og ikke `netplan apply` over SSH. Så ryger forbindelsen ikke permanent, hvis noget er forkert.

**Ingen DNS / kan ikke `apt update`**
```bash
resolvectl status
ping -c3 1.1.1.1        # virker IP?
ping -c3 ubuntu.com     # virker DNS?
```

## Disk

**Disken er fuld**
```bash
df -h
sudo ncdu /             # find de store mapper
docker system df
docker system prune     # fjerner stoppede containere, ubrugte netværk og dangling images
sudo journalctl --vacuum-size=200M
```

**`/` er meget mindre end disken** → se [[05 - Grundopsætning og sikkerhed#3. Udvid disken (hvis jeg glemte det)]].

## Docker

**`permission denied while trying to connect to the Docker daemon socket`**
Jeg er ikke i `docker`-gruppen endnu, eller jeg har ikke logget ud og ind siden. `groups` skal vise `docker`.

**Container starter ikke**
```bash
docker compose ps
docker compose logs --tail 100 <service>
```

**Port allerede i brug**
```bash
sudo ss -tulpn | grep :3001
```

## Tailscale

```bash
tailscale status
sudo systemctl status tailscaled
tailscale ping homeserver     # fra min primære PC
sudo tailscale up             # log ind igen, hvis nøglen er udløbet
```

## Claude Code

```bash
claude doctor
which claude                  # findes den ikke → ~/.local/bin mangler i PATH
```

## Nyttige log-kommandoer

```bash
journalctl -b -p warning      # advarsler og fejl siden opstart
journalctl -u ssh -f          # følg logs for en bestemt service
dmesg -T | tail -50           # kernel/hardware-beskeder
```
