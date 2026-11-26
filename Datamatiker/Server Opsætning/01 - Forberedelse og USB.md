---
tags: [server, linux, homelab]
---

# 01 – Forberedelse og USB

← [[00 - Oversigt]] | Næste: [[02 - Installer Ubuntu Server]] →

## Det skal jeg bruge

- Mini PC'en + strømforsyning
- Skærm (HDMI/DisplayPort) og USB-tastatur (kun til starten)
- **Netværkskabel** – helst kablet forbindelse til routeren. WiFi kan godt sættes op i installeren, men kabel er mere stabilt til en server.
- USB-stik på mindst 4 GB (bliver slettet)
- Min primære PC

## 1. Hent Ubuntu Server

1. Gå til <https://ubuntu.com/download/server>.
2. Hent den nyeste **LTS**-udgave (Long Term Support, 5 års opdateringer). Lige nu er det **Ubuntu 26.04 LTS**.
   - Vælg ikke mellemudgaverne (fx 25.10), de har kun 9 måneders support.
3. Kontrollér filen (valgfrit, men en god vane). Kør i mappen med ISO-filen:

```bash
sha256sum ubuntu-*-live-server-amd64.iso
```

Sammenlign resultatet med `SHA256SUMS` på download-siden.

## 2. Lav en bootbar USB

### Mulighed A – `dd` (indbygget i Linux)

> [!danger] Vælg det rigtige drev
> `dd` overskriver alt på det drev, jeg peger på, uden at spørge. Tjek drevet to gange.

```bash
# Find USB-stikket – kig efter størrelsen og TRAN = usb
lsblk -o NAME,SIZE,TRAN,MODEL

# Eksempel: USB-stikket er /dev/sdb (IKKE /dev/sdb1)
sudo dd if=ubuntu-26.04-live-server-amd64.iso of=/dev/sdb bs=4M status=progress oflag=sync
```

### Mulighed B – grafisk program

- **Impression** eller **balenaEtcher**: vælg ISO-filen, vælg USB-stikket, tryk Flash.
- **Ventoy**: installér Ventoy på stikket én gang, og kopiér derefter bare ISO-filer over. Praktisk, hvis jeg vil have flere systemer på samme stik.

## 3. BIOS/UEFI på mini PC'en

Tænd mini PC'en og tryk på BIOS-tasten under opstart (typisk **Del**, **F2** eller **F7**, se producentens side). Boot-menuen er ofte **F7**, **F11** eller **F12**.

Indstillinger:

| Indstilling | Værdi | Hvorfor |
|-------------|-------|---------|
| Boot order / Boot menu | USB først (eller vælg den én gang i boot-menuen) | Så installeren starter |
| **Restore on AC Power Loss** / *Power On after power failure* | **Power On** | Serveren tænder selv igen efter strømsvigt |
| Secure Boot | Kan blive slået til | Ubuntu understøtter Secure Boot |
| Wake on LAN (valgfrit) | Enabled | Så serveren kan tændes over netværket |
| Fast Boot | Disabled (anbefalet) | Gør det nemmere at komme i BIOS senere |

Navnene er forskellige fra producent til producent. Kig efter noget med "Power", "AC" eller "After Power Loss".

## 4. Router (kan også gøres senere)

Find routerens admin-side (typisk `192.168.1.1` eller `192.168.0.1`). Når serveren er installeret, laver jeg en **DHCP-reservation**, så den altid får samme IP. Det er beskrevet i [[03 - Første boot og SSH]].

---

Næste: [[02 - Installer Ubuntu Server]] →
