---
tags: [server, linux, homelab, docker]
---

# 08 – Docker

← [[07 - Tailscale]] | Næste: [[09 - Claude Code]] →

Docker installeres fra **Dockers eget apt-repository**. Ikke `docker.io` fra Ubuntu (gammel version) og ikke snap (problemer med rettigheder og mapper).

## 1. Fjern eventuelle gamle/konfliktende pakker

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
  sudo apt remove -y $pkg 2>/dev/null
done
sudo snap remove docker 2>/dev/null
```

## 2. Tilføj Dockers repository

```bash
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources > /dev/null <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

> [!note] Hvis `apt update` fejler med Docker-linjen
> Lige efter en ny Ubuntu-udgave er udkommet, har Docker ikke altid pakker klar til den. Tjek den officielle guide: <https://docs.docker.com/engine/install/ubuntu/>

## 3. Installér Docker Engine + Compose

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
```

## 4. Brug Docker uden sudo

```bash
sudo usermod -aG docker $USER
```

Log ud og ind igen (`exit` → `ssh homeserver`), og test så:

```bash
docker run --rm hello-world
docker compose version
```

> [!warning]
> Medlemmer af `docker`-gruppen har reelt root-adgang til systemet. Det er fint på min egen server, men giv det ikke til andre brugere.

## 5. Begræns logfilernes størrelse

Containerlogs kan som standard vokse, til disken er fuld. Sæt en grænse:

```bash
sudo micro /etc/docker/daemon.json
```

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

```bash
sudo systemctl restart docker
```

(Gælder kun for containere, der oprettes efter ændringen.)

## 6. Mappestruktur til tjenester

Én mappe pr. tjeneste, hver med sin egen `compose.yaml`. Gerne i et git-repo, så konfigurationen er versioneret:

```
~/docker/
├── uptime-kuma/
│   └── compose.yaml
├── dockge/
│   └── compose.yaml
└── ...
```

```bash
mkdir -p ~/docker && cd ~/docker
git init
```

> Læg aldrig passwords i git. Brug en `.env`-fil ved siden af `compose.yaml`, og skriv `.env` i `.gitignore`.

## 7. Første rigtige tjeneste – eksempel: Uptime Kuma

```bash
mkdir -p ~/docker/uptime-kuma && cd ~/docker/uptime-kuma
micro compose.yaml
```

```yaml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:2
    container_name: uptime-kuma
    restart: unless-stopped
    ports:
      - "3001:3001"
    volumes:
      - ./data:/app/data
```

```bash
docker compose up -d
docker compose logs -f      # Ctrl+C for at stoppe visningen
```

Åbn `http://homeserver:3001` på min primære PC (via Tailscale).

### Daglige Docker-kommandoer

| Kommando | Hvad |
|----------|------|
| `docker compose up -d` | Start (i baggrunden) |
| `docker compose down` | Stop og fjern containere (data i volumes bliver liggende) |
| `docker compose pull && docker compose up -d` | Opdatér til nyeste image |
| `docker compose logs -f` | Følg logs |
| `docker ps` | Kørende containere |
| `docker exec -it <navn> sh` | Shell inde i en container |
| `docker system df` | Hvor meget fylder Docker |
| `docker image prune -a` | Ryd op i ubrugte images |

## Docker og firewall

> [!danger] Docker går uden om UFW
> En port i `ports: - "3001:3001"` er åben for **hele det lokale netværk**, også selvom UFW blokerer den. Docker laver sine egne iptables-regler.

Muligheder:

1. **Kun lokalt på serveren** (fx bag en reverse proxy):
   ```yaml
   ports:
     - "127.0.0.1:3001:3001"
   ```
2. **Kun via Tailscale** – bind til serverens Tailscale-IP (`tailscale ip -4`):
   ```yaml
   ports:
     - "100.x.y.z:3001:3001"
   ```
3. Hjemmenetværket stoler jeg på, og ingen porte er åbne i routeren → standard (`"3001:3001"`) er okay.

## Valgfrit: lazydocker (TUI til Docker)

```bash
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash
lazydocker
```

Viser containere, logs, CPU/RAM og giver mulighed for at genstarte containere med tastaturet.

---

Næste: [[09 - Claude Code]] →
