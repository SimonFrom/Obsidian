---
tags: [server, linux, homelab, sikkerhed]
---

# 05 – Grundopsætning og sikkerhed

← [[04 - Herdr]] | Næste: [[06 - Grundværktøjer og Git]] →

Alt herfra foregår over SSH og inde i herdr: `herdr --remote homeserver` fra min primære PC (eller `ssh homeserver` → `herdr`).

## 1. Opdatér systemet

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt autoremove -y
```

Hvis der står *System restart required*, når jeg logger ind:

```bash
sudo reboot
# vent ca. 30-60 sek, og log ind igen
ssh homeserver
```

## 2. Tidszone

```bash
sudo timedatectl set-timezone Europe/Copenhagen
timedatectl    # tjek at Time zone og "System clock synchronized: yes" står rigtigt
```

## 3. Udvid disken (hvis jeg glemte det)

Tjek, hvor meget plads `/` har:

```bash
df -h /
sudo vgs        # "VFree" viser, hvor meget plads der ikke er brugt
```

Er `VFree` mere end 0, så giv al pladsen til root:

```bash
sudo lvextend -r -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
df -h /         # nu skal størrelsen passe med disken
```

(`-r` udvider også filsystemet med det samme.)

## 4. Hærd SSH

Når nøgle-login virker ([[03 - Første boot og SSH]]), slår jeg password-login og root-login fra.

> [!danger] Hold en SSH-session åben
> Hav én terminal med en aktiv SSH-forbindelse åben, mens jeg ændrer SSH. Går noget galt, kan jeg rette det derfra.

```bash
sudo micro /etc/ssh/sshd_config.d/01-hardening.conf
# micro er ikke installeret endnu? Brug nano indtil videre:
# sudo nano /etc/ssh/sshd_config.d/01-hardening.conf
```

Indhold:

```
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitRootLogin no
PubkeyAuthentication yes
```

> [!info] Hvorfor `01-` i filnavnet?
> SSH bruger den **første** værdi, den møder, og filerne i `sshd_config.d/` læses i alfabetisk rækkefølge. Ubuntu laver nogle gange `50-cloud-init.conf` med `PasswordAuthentication yes`. Hvis min fil hed `99-...`, ville den blive ignoreret.

Tjek og genstart:

```bash
sudo sshd -t                                   # ingen output = ingen fejl
sudo systemctl restart ssh
sudo sshd -T | grep -Ei 'passwordauth|permitroot'
# Forventet:
# permitrootlogin no
# passwordauthentication no
```

Test i en **ny** terminal på min primære PC, mens den gamle stadig er åben:

```bash
ssh homeserver                                  # skal virke (nøgle)
ssh -o PubkeyAuthentication=no homeserver      # skal give "Permission denied"
```

## 5. Firewall (UFW)

```bash
sudo ufw allow OpenSSH
sudo ufw enable           # svar y
sudo ufw status verbose
```

Standard er: alt indgående blokeret, alt udgående tilladt. Senere åbner jeg kun for det, jeg skal bruge, fx:

```bash
sudo ufw allow 80/tcp     # eksempel – kun når jeg rent faktisk har en webserver
```

> [!warning] Docker og UFW
> Porte, som Docker åbner med `ports:`, går **uden om** UFW. Se [[09 - Docker#Docker og firewall]].

## 6. Automatiske sikkerhedsopdateringer

`unattended-upgrades` følger normalt med Ubuntu Server. Tjek og slå til:

```bash
sudo apt install -y unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades   # vælg "Yes"
```

Automatisk genstart om natten, når en opdatering kræver det (valgfrit):

```bash
sudo micro /etc/apt/apt.conf.d/52unattended-upgrades-local
```

```
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "04:00";
```

Test (ændrer ikke noget):

```bash
sudo unattended-upgrade --dry-run --debug | tail -20
```

## 7. Fail2ban (valgfrit)

Med password-login slået fra og ingen åbne porte i routeren er fail2ban ikke nødvendigt. Hvis SSH en dag åbnes mod internettet:

```bash
sudo apt install -y fail2ban
sudo systemctl enable --now fail2ban
sudo fail2ban-client status sshd
```

---

Næste: [[06 - Grundværktøjer og Git]] →
