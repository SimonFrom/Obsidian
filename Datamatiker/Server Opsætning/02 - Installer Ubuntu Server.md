---
tags: [server, linux, homelab]
---

# 02 – Installer Ubuntu Server

← [[01 - Forberedelse og USB]] | Næste: [[03 - Første boot og SSH]] →

> [!info] Skærm og tastatur
> Det her trin kræver skærm og tastatur på mini PC'en. I installeren bruger jeg **piletasterne**, **Tab**, **Mellemrum** (sæt/fjern kryds) og **Enter**.

Sæt USB-stikket i, tænd mini PC'en og boot fra USB (via boot-menuen eller boot order).

## 1. GRUB-menuen

Vælg **Try or Install Ubuntu Server**.

## 2. Sprog

Vælg **English**.

> [!tip] Hvorfor engelsk?
> Fejlbeskeder og logfiler er så på engelsk, så jeg kan søge direkte på dem. Det er også det, alle guides og Claude forventer. Tastaturet sætter jeg til dansk i næste trin.

Hvis installeren tilbyder at opdatere sig selv (*Installer update available*): vælg **Update to the new installer**.

## 3. Tastatur

- Layout: **Danish**
- Variant: **Danish**

Skriv lidt i testfeltet og tjek, at `æ ø å | ~ /` virker.

## 4. Type af installation

Vælg **Ubuntu Server** (ikke *minimized*). Den minimale udgave mangler mange almindelige værktøjer, og når jeg ikke selv skal vedligeholde et kæmpe system, er det ikke besværet værd.

Sæt **ikke** kryds i *Search for third-party drivers*, medmindre der er hardware, som ikke virker.

## 5. Netværk

- Med netværkskabel får forbindelsen (fx `enp1s0`) en IP via DHCP. Skriv IP'en ned, fx 
  ```
  192.168.0.171/24
  ```
- Uden kabel: vælg WiFi-kortet (`wlp...`), og skriv netværksnavn og kode.
- Lad den stå på **DHCP**. En fast IP laver jeg med en reservation i routeren (se [[03 - Første boot og SSH]]). Det er nemmere og sværere at lave fejl i.

## 6. Proxy

Lad feltet være tomt → **Done**.

## 7. Ubuntu Archive Mirror

Lad den finde et spejl (ofte `dk.archive.ubuntu.com`), og vent på, at testen bliver færdig → **Done**.

## 8. Lager (storage)

1. Vælg **Use an entire disk** og den rigtige disk (den interne SSD/NVMe – **ikke** USB-stikket).
2. Behold kryds i **Set up this disk as an LVM group**.
3. Kryptering (*Encrypt the LVM group with LUKS*): **nej**, medmindre jeg vil skrive et password ved hver opstart. Med kryptering kan serveren ikke starte selv efter strømsvigt.

### ⚠️ Vigtigt: Ubuntu bruger ikke hele disken som standard

På oversigtsskærmen bliver `ubuntu-lv` (`/`) ofte sat til kun en del af disken (typisk 100 GB eller halvdelen), og resten står som *free space*.

**Ret det med det samme:**

1. Under *USED DEVICES* går jeg til `ubuntu-lv` → **Edit**.
2. Sæt **Size** til det maksimale (der står en max-værdi ud for feltet).
3. **Save** → **Done** → **Continue** (bekræft at disken slettes).

> Har jeg glemt det, kan det rettes bagefter, se [[05 - Grundopsætning og sikkerhed#3. Udvid disken (hvis jeg glemte det)]].

## 9. Profil

| Felt | Eksempel |
|------|----------|
| Your name | Simon |
| Your server's name | `homeserver` |
| Pick a username | `simon` |
| Password | Et stærkt password, som jeg kan huske – det bruges til `sudo` |

## 10. Ubuntu Pro

Vælg **Skip for now**. (Ubuntu Pro er gratis til private og kan slås til senere med `sudo pro attach`, men er ikke nødvendigt.)

## 11. SSH – vigtigt!

- ✅ Sæt kryds i **Install OpenSSH server**.
- **Import SSH key** (valgfrit, men smart): vælg **from GitHub** og skriv mit GitHub-brugernavn. Så bliver de offentlige nøgler fra min GitHub-konto lagt på serveren, og jeg kan logge ind med nøgle med det samme.
  - Har jeg ikke min primære PC's nøgle på GitHub, springer jeg det over og tilføjer nøglen i næste trin.
- *Allow password authentication over SSH*: lad den stå **til** for nu. Den slås fra, når nøgle-login virker.

## 12. Featured server snaps

**Vælg ingen.** Især **ikke docker** herfra – snap-udgaven af Docker giver problemer med rettigheder og mapper. Docker installeres fra Dockers egen kilde i [[09 - Docker]].

## 13. Installation

Vent til der står **Installation complete!** Der står måske *Downloading and installing security updates* – lad den blive færdig.

1. Vælg **Reboot Now**.
2. Fjern USB-stikket, når der står *Please remove the installation medium* → tryk **Enter**.

---

Næste: [[03 - Første boot og SSH]] →
