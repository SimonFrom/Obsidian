# AirPlate — System Architecture (praktik overview)

> A single reference to understand how the three repos in this folder —
> **`AirPlateServer`**, **`app`**, and **`dashboard-core`** — fit together.
> Written for onboarding, so it goes into the technical detail: services,
> routes, endpoints, auth, databases, and the data flow that ties them all
> together.

---

## 1. What is AirPlate?

AirPlate is a **drone-detection / counter-UAS (Unmanned Aircraft System) platform**.
Physical **scanners / sensors** ("AirPlates") sit in the field and listen for the
radio signals that drones and their controllers broadcast (Remote ID, DJI OcuSync,
and RF geolocation from the MyDefence *Argos* hardware). Those detections are
streamed to a backend, stored, enriched (which manufacturer? which model? inside
an alarm zone?), and shown to two very different audiences:

- **Customers** — via the **`app`** frontend: a live map of drones, an intrusion
  log, analytics dashboards, notifications.
- **AirPlate staff / operators** — via the **`dashboard-core`** internal console:
  fleet health, organizations, users, firmware deploys, database admin, email
  campaigns, CI/CD.

The company/tenant is called an **Organization**. Almost everything in the system
is scoped by `organization_id` — a customer only ever sees their own drones,
scanners, and users. This is the single most important concept in the codebase.

---

## 2. The three projects at a glance

| Folder | Role | Stack | Who uses it |
|--------|------|-------|-------------|
| **`AirPlateServer`** | The backend. A **monorepo of microservices** that ingest sensor data, store it, and serve it over REST + WebSocket + SSE. | Bun + TypeScript, Elysia (HTTP API), Docker Compose, MySQL + InfluxDB + PostgreSQL, NATS | Everything else talks to it |
| **`app`** | The **customer-facing web dashboard** (SPA). | React 18 + TypeScript + Vite 5, Zustand, React Query, Mapbox GL, Firebase Auth | Paying customers |
| **`dashboard-core`** | The **internal staff/ops console**, built as a pluggable shell that auto-discovers "panels". | Astro 5 (SSR) + Svelte 5, Bun, Redis, Tailwind 4 | AirPlate employees (`@airplate.dk`) |

### How they connect (high level)

```
                          ┌───────────────────────────────────────┐
   Field hardware         │            AirPlateServer             │
  ┌───────────────┐  RF/  │  (backend monorepo, ws.airplate.dk)   │
  │  Scanners /   │  UDP/  │                                       │
  │  AirPlates    │──TCP──▶│  ingest → InfluxDB (telemetry)        │
  │  (Remote ID,  │        │        → MySQL (users/orgs/detections)│
  │   OcuSync)    │        │        → PostgreSQL (analytics mirror)│
  └───────────────┘        │                                       │
  ┌───────────────┐  VPN   │  Elysia REST API  (:5443, TLS)        │
  │ MyDefence     │──────▶ │  WebSocket gateway (:3000 /scanning)  │
  │ Argos server  │ (Argos │  Scanner SSE      (:3010 firehose)    │
  └───────────────┘  RF)   └───────┬───────────────────┬───────────┘
                                   │ REST + WSS         │ REST proxy + SSE
                                   │ (Firebase cookie   │ (server-to-server,
                                   │  or api-key)       │  shared docker net)
                         ┌─────────▼─────────┐  ┌────────▼──────────────┐
                         │       app         │  │    dashboard-core     │
                         │  (customer SPA)   │  │  (staff console,      │
                         │  app.airplate.dk  │  │   /monitoring, /dash) │
                         └───────────────────┘  └───────────────────────┘
```

Both frontends ultimately read from **the same backend and the same MySQL
database**. The difference is *who* logs in and *what slice* of data they are
allowed to see.

---

## 3. Repo #1 — `AirPlateServer` (the backend)

This is the heart of the system and by far the largest repo. It is a **Docker
Compose monorepo**: one `docker-compose.yml` at the root wires together ~25
services. Each subfolder is an independently deployable piece.

### 3.1 The services (from `docker-compose.yml`)

| Service | What it does |
|---------|--------------|
| **`api`** | The main **REST API** (Elysia on Bun). Serves `ws.airplate.dk:5443`. This is what `app` and `dashboard-core` call. See §3.3. |
| **`websocket`** | The live **WebSocket gateway** (`/scanning`, `/droneids`). Pushes real-time drone positions to the customer map. |
| **`airplate-ingest`** | Receives raw telemetry from scanners over **UDP (7070) / TCP (8082)** and writes it to InfluxDB. |
| **`airplate-ws`** | Owns the WSS/TLS `/droneids` broadcast on **:3000**. Split out from ingest so a cert renewal only restarts this. They share `AIRPLATE_WS_INGEST_SECRET` for the internal `/publish` POST. |
| **`scanner-sse`** | **Server-Sent Events** firehose of scanner status across *all* orgs on **:3010** (`/internal/events/scanners`). `dashboard-core` proxies this. |
| **`scanner-servers`** | The TCP/UDP servers the physical scanners connect into. |
| **`intrusion-detector`** | Watches detections and raises intrusions when a drone enters an alarm zone. |
| **`alert-manager`** | Fans out alerts/notifications (push, email, WhatsApp, signal). |
| **`notification` (+ mail/whatsapp/signal-service)** | Delivery channels for alerts. |
| **`flight-session-svc`** | Groups raw detections into **flight sessions / flight logs**; also writes to the PostgreSQL mirror. |
| **`eos-live` / `eos-jobs` / `eos-sync` / `eos-postgres`** | The **EOS** subsystem — Environment/telemetry timeseries jobs backed by its own Postgres + NATS. |
| **`backfill`** | Internal Bun API (:4400) that re-processes historical data. `dashboard-core`'s Organizations panel proxies to it. |
| **`health-monitor`** | Health checks; `autoheal` restarts unhealthy containers. |
| **`cert-api`** | Serves TLS certs to other services (`CERT_API_URL`) so they don't each read `/etc/letsencrypt`. |
| **`traefik`** | Reverse proxy / TLS termination on the dev host. |
| **`tcp-ota`** | Over-the-air firmware updates pushed to AirPlate devices (`tcpSoftwareOverTheAir.js`). |
| **`pi-ota-server`** | OTA server for Raspberry-Pi–based sensors. |
| **`log-rotator`**, **`nats`**, **`simulator`** | Log rotation, the NATS message bus, and a drone simulator for testing. |

Supporting non-service folders: **`MYSQL/`** (self-hosted MySQL fallback for
`mysql65.unoeuro.com` + phpMyAdmin), **`POSTGRES/`** (the analytics mirror),
**`influx/`** (InfluxDB client), **`mydefenceClient/`** (VPN bridge to the
MyDefence Argos server — see §3.6), and **`dashboard-core/`** as a git submodule.

### 3.2 The datastores

The backend is **polyglot-persistence**: it uses three databases, each for what
it's good at.

| Store | Holds | Notes |
|-------|-------|-------|
| **MySQL** (`airplate_dk_db_*`) | The relational "system of record": `Organizations`, `Users`, `Users_Organizations` (membership), `ApiKeys`, `Invitees`, `Membership_Audit`, approved drones, alarm zones, scanners, flight logs, settings. | Hosted at `mysql65.unoeuro.com` (Simply.com) in prod, with a self-hosted Docker MySQL as emergency fallback. Both frontends read from this. |
| **InfluxDB** (:8086, `airplate` org) | **Time-series telemetry** — the raw high-volume drone position/RF stream from scanners (bucket `droneid`). Also an `observer` bucket for ops metrics. | This is where the "live" data lands first. |
| **PostgreSQL 18** (`eos-postgres` + the `POSTGRES/` mirror) | **Analytics / spatial** mirror of MySQL (PostGIS), plus the EOS timeseries. Fed by `flight-session-svc`; the `mysql_id` column ties a Postgres row back to its MySQL origin. | *Not* a production DB — analytics + future-migration exploration only. |

### 3.3 The REST API — structure & endpoints

The API lives in `main/api/` and is built with **Elysia**. `app.ts` composes the
whole server by `.use()`-ing one plugin per **domain** (`main/api/domains/*`).
`server.ts` boots it: HTTPS on `:5443` in deployment/testing, plain HTTP on `:80`
locally, certs from `cert-api` or `/etc/letsencrypt`.

> **Note on the two route folders.** `main/api/domains/*` is the current Elysia
> API. `main/routes/*` (detections, scans, remarks, settings, approvedDrones) is
> an older/parallel set being folded in — the codebase is mid-migration from a
> legacy Express server (`httpsServer.js`) into the Elysia domains. There's also
> an OpenAPI/Scalar docs UI (`openapi.ts`) exposed internally via `OPENAPI_PORT`.

The domains, and the kind of endpoints each owns:

| Domain | Responsibility (representative endpoints) |
|--------|-------------------------------------------|
| **auth** | `login`, `logout`, `logout-everywhere`, `authenticate`, TOTP setup/verify (`auth/setup-totp`, `auth/verify-totp`, `auth/verify-login-totp`, `auth/remove-totp`) |
| **passkey** | WebAuthn register/login options + verify (backed by the `WebAuthn_Credentials` table) |
| **users** | `registerUser`, `registerFirebaseUser`, `completeInvite`, `deleteUser`, `editUser`, profile, memberships, `sendPasswordResetEmail` |
| **organizations** | `createOrganization`, `editOrganization`, `myOrganizations`, `selectOrganization`, `organizationId/*`, `organizationNameFromId/*`, invitees (`sendInvite`, `inviteePrecheck`, `inviteeStatus`, `invitedUsers`, `deleteInvitee`, `acceptInviteAsExistingUser`) |
| **airplates** | `createAirplate`, `editAirplate`, `editAirplateOrganization`, `airplateFlags/*`, `editAirplateFlags` (device/unit management) |
| **scanners** | `organizationScanners`, `organizationScannerImeis`, `isScannerMainOwner/*`, `editScannerName`, `editScannerAutoUpdate`, `editScannerGnssDisable` |
| **drones / approved-drones** | `createDrone`, `editDrone`, `deletedrone`, `editDroneIDs`, `organization-drones`, approved-drone allow-list CRUD |
| **detections** | The core read path: `scannedDevices`, `todaysScannedDevices`, encrypted detections, remarks (`airdetect-detections/:id/remarks`), plus an SSE stream |
| **coordinates** | `coordinates`, `coordinatesFromMac`, `coordinatesFromMacReplay`, `operatorCoordinates`, `ocusycOperatorCoordinates`, `droneFlightStart` — the position/track data the live map draws |
| **flight-logs** | `flight-logs`, `get_flight_log`, `get_flight_logs/:imei`, `flight-logs-with-coordinates`, `flight-logs-csv-data`, `downloadTrack`, `update_flight_uas_id`, `update_flight_user_id`, `flight-log/delete` |
| **stats** | Heatmaps and counts: `heatmapData`, `calendarHeatmapData`, `scanCount`, `scanAverage`, `scanDays`, `topScannedUasIds`, `topScannedOperatorIds`, and the `subscription*` variants |
| **alarm-zones / geo-zones** | `organization-alarm-zones`, `trackZones`, `getSubscribedZones`, `zoneToggle`, `subscribeZone`, and static GeoJSON overlays at `geo/:id` (e.g. drone-zones) |
| **argos** | MyDefence Argos RF data: `argos-data`, `argos-todays-data`, `argos-rf-geolocation-data` |
| **faa-rid** | FAA Remote ID lookup — enriches a detection's serial into make/model (cached in-process, auto-refreshing) |
| **notifications** | `notification-contacts` CRUD + links, `notificationToggle`, `updateNotificationToggle` |
| **prefs / settings** | `preferredMapStyle`, `updateMapStyle`, `settings/scannerCalibration` |
| **stripe** | Subscriptions & billing: `create-checkout-session`, `checkIfHasSubscriptions`, `createSubscriber`, webhooks |
| **email** | Transactional email (via `resend`), `newSubscriberEmail` |
| **kml** | KML exports: `intrusion-download-kml` |
| **i18n** | Serves locale/translation files |
| **misc** | `countryCodes`, public stats, `get-shipping-price` (Shipmondo), OPRN validation (`validateOprn/*`), etc. |

### 3.4 The authentication model (very important)

The backend accepts **two credential types**, resolved in `main/api/middleware/auth.ts`:

1. **Firebase session cookies** (`access_token` + `refresh_token`) — this is how a
   human logs in. The `app` frontend authenticates with **Firebase Auth**
   (project `airplate-4062f`), and the backend verifies the Firebase ID token
   with `firebase-admin` (`verifyIdToken(token, checkRevoked=true)`).
   - `access_token` lives 12 h, `refresh_token` 30 days.
   - **Continuous login:** if the access token expired but the refresh token is
     still valid, the middleware silently mints a new one and *slides* the
     30-day window — active users never get logged out. Requests are
     de-duplicated (single-flight) and cached in a 30 s LRU so a burst of calls
     only triggers one Firebase verification.
   - `logout-everywhere` / password reset call `revokeRefreshTokens`, which the
     revocation check catches and forces a full re-login.
2. **API keys** (`api-key` header) — machine-to-machine. Validated against the
   MySQL `ApiKeys` table, which resolves directly to an `organization_id`.

On top of that is **multi-org resolution**: a user can belong to several
Organizations (`Users_Organizations`), and `selectOrganization` sets the "current"
one. Every authed request is pinned to one org, and there's a background invariant
check that emails an operator if a `Users` row ever points at an org with no
matching membership row.

**Optional second factors:** TOTP (`speakeasy` + QR) and **passkeys / WebAuthn**
(`@simplewebauthn/server`, RP id `ws.airplate.dk`).

### 3.5 Real-time paths (WebSocket + SSE)

Live data does *not* go over REST:

- **Customer live map** → the `app` opens a **WebSocket** to
  `${WSurl}scanning?organization_id=X` (the `websocket` / `airplate-ws` service).
  Each message creates/updates a drone + operator icon on the Mapbox map.
- **Scanner fleet status** → `scanner-sse` exposes an all-orgs SSE firehose at
  `/internal/events/scanners` (:3010). Only internal callers with
  `SCANNER_SSE_INTERNAL_SECRET` may open it; `dashboard-core` proxies it behind
  its own login.

### 3.6 MyDefence / Argos integration

The RF-geolocation data comes from **MyDefence's Argos hardware**, which lives on
MyDefence's internal network (`10.230.230.0/24`). The **`mydefenceClient`** service
is a Dockerized **OpenVPN bridge + Node client** that connects into that network,
pulls Argos detections, and feeds them into AirPlate. The `argos` API domain then
serves that data to the frontend. (This is why the product is sometimes branded
"MyDefence" — see the `mydefence` build/deploy targets.)

---

## 4. Repo #2 — `app` (the customer frontend)

A **React 18 + TypeScript SPA** built with **Vite 5**, using **Bun** as the
runtime/package manager.

### 4.1 Stack

- **State:** Zustand for client state (`droneMapStore`), React Query for all
  server data.
- **Maps:** Mapbox GL JS v2 — the live drone map and heatmaps.
- **Auth:** Firebase Auth (`src/firebaseConfig.tsx`, project `airplate-4062f`) —
  the *same* Firebase project the backend verifies against.
- **Routing:** React Router v6. Pages in `src/pages/` (`Dashboard`, `MapView`,
  `IntrusionLog`, `Login`).
- **i18n / branding:** multi-client — `VITE_CLIENT_ID=default | mydefence` swaps
  logos/theme so the same code ships as AirPlate *or* MyDefence.

### 4.2 How it talks to the backend

Everything funnels through **`src/APIClient.ts`** (~2,600 lines), which exports two
base URLs:

- **`APIurl`** — the REST base, from `VITE_API_HOST` / `VITE_API_ENV`.
- **`WSurl`** — the WebSocket base, independent (`VITE_WS_HOST`), so you can point
  the map at prod WS while hitting a dev API.

| `VITE_API_ENV` | REST API | WebSocket |
|----------------|----------|-----------|
| `production` | `ws.airplate.dk` | `ws.airplate.dk` |
| `development` | `development.airplate.dk` | `development.airplate.dk` (via Caddy) |
| (unset / local) | `localhost:80` | `localhost:80` |

`APIClient.ts` wraps every backend endpoint from §3.3 as a typed function. Requests
carry the Firebase session cookie (credentials are included), so the browser never
handles the token directly — login sets the httpOnly cookie server-side.

### 4.3 Live drone map flow (the signature feature)

1. WebSocket connects to `${WSurl}scanning?organization_id=X`.
2. Incoming messages create/update drone + operator icons on Mapbox; a dashed
   yellow line always joins a drone to its operator.
3. Clicking an icon fetches trails via the `coordinates*` REST endpoints (blue =
   drone track, red = operator track).
4. Navigating from the Dashboard or a notification stashes a `focusDrone` in the
   Zustand store so the map can draw immediately from the captured WS message
   without a reload.

Notifications are dual: a **toast** (react-toastify) on each detection and a
persistent **bell** list in the top nav (localStorage-backed).

### 4.4 Deployment

FTP-based deploy to **Simply.com** static hosting (`scripts/deploy`). Targets map a
build to a remote dir + domain:

| Command | Domain | REST API it points at |
|---------|--------|-----------------------|
| `bun deploy:dev` | `dev.airplate.dk` | `development.airplate.dk` |
| `bun deploy:app` | `app.airplate.dk` | `ws.airplate.dk` |
| `bun deploy:fr` | `fr.airplate.dk` | `ws.airplate.dk` |
| `bun deploy:mydefence` | `mydefence.airplate.dk` | `ws.airplate.dk` |

So `app` is a pure static bundle; all its intelligence about *which backend* it
talks to is baked in at build time via Vite env vars.

---

## 5. Repo #3 — `dashboard-core` (the internal staff console)

An **Astro 5 SSR** app (Svelte 5 islands, Bun, Redis, Tailwind 4) that is *not* a
single app but a **shell + plugin system**. It's what AirPlate employees use to run
the platform.

### 5.1 The panel architecture

`dashboard-core` is a thin shell providing **auth, layout, and discovery**. The
actual features are **panels** — each panel is a separate module (in this checkout,
under `panels/<name>/dashboard-panel/`, added as git submodules in the real repo).

At build time `panelDiscovery.ts` scans siblings for a
`dashboard-panel/panel.config.ts` and auto-injects that panel's routes, vite
aliases, and optional server-boot hooks into the Astro app. A panel declares:

```ts
export default {
  name, label, description, icon,
  routes: [{ pattern: "/observer", entrypoint: "./src/pages/index.astro" }, ...],
  viteAliases: { "@observer": "." },
  serverInit: "./src/lib/sampler.ts", // optional background worker
} satisfies PanelConfig;
```

The shell alias `@shell` gives every panel access to the shared layout, session,
and fetch helpers. Panels are wired in via a `virtual:panels` module.

### 5.2 The panels (what staff can do)

| Panel (`name`) | Label | What it does | Example API routes |
|----------------|-------|--------------|--------------------|
| `airplates` | AirPlates | Manage AirPlate units + push firmware binaries (OTA) | `/api/airplates/binary/[env]/upload`, `/api/airplates/[id]` |
| `sensors` | Sensors | Scanner/sensor fleet + OTA binary + Raspberry-Pi deploys | `/api/sensors/binary/[env]/upload`, `/api/sensors/[imei]`, `/api/sensors/sse/scanners` |
| `scanner-fleet` (mail-service) | Fleet Monitor | Live scanner health dashboard, ML analytics, telemetry | `/api/dashboard`, `/api/scanners`, `/api/sse/scanners`, `/api/analysis`, `/api/scanner-telemetry` |
| `organizations` | Organizations | CRUD orgs, scanners per org, invitees, member privileges, trigger backfills | `/api/organizations`, `/api/organizations/[id]/scanners`, `/api/organizations/[id]/backfill` |
| `users` | Users | User admin + org memberships | `/api/users`, `/api/users/[id]/memberships` |
| `observer` | Observer | Ops observability — services, logs, resources, traffic (samples container metrics + Influx) | `/api/observer/overview`, `/api/observer/logs`, `/api/observer/traffic` |
| `cicd` | CI/CD | Trigger GitHub deploy workflows + view runs | `/api/cicd/deploy`, `/api/cicd/runs` |
| `docs` | Docs | Renders the backend's OpenAPI spec (Scalar) + software docs | `/api/docs/openapi.json`, `/docs/api` |
| `email-builder` | Email Builder | Visual email/campaign builder + batch send to contact books | `/api/email-builder/templates`, `/api/email-builder/send-batch` |
| `web-builder` | Web Builder | Drag-and-drop page builder (Rust/WASM `builder-ui`) | (UI panel) |
| `phpmyadmin` | Database Admin | Embedded phpMyAdmin over the MySQL DB | `/phpmyadmin` |

### 5.3 How dashboard-core authenticates & authorizes

Two layers, both in `src/middleware.ts`:

1. **Session gate.** Its own login (`/api/auth/login`, TOTP, passkeys) sets a JWT
   session cookie (Redis-backed). Any request without `session.userId` →
   redirect to `/login`; without `session.totpVerified` → TOTP challenge.
2. **Staff gate.** Sensitive prefixes (`/organizations`, `/users`, `/sensors`,
   `/airplates`, `/cicd`, `/observer`, `/docs` and their `/api/*` twins) are
   **staff-only** — you must have an `@airplate.dk` email (or be in
   `STAFF_EMAIL_ALLOWLIST`), enforced by `isAirplateStaff()` in `src/lib/auth.ts`.
   Non-staff get a hard `403` before any panel logic runs.

### 5.4 How dashboard-core reaches the backend

`dashboard-core` **does not have its own database of drones** — it proxies to
`AirPlateServer`:

- **`src/lib/api-config.ts`** resolves `AUTH_BACKEND_URL` (prod → `ws.airplate.dk`,
  dev → `development.airplate.dk:5443`). Its `/api/auth/*` endpoints proxy the
  login to that backend.
- **`src/lib/backendFetch.ts`** is a server-to-server fetch that attaches the
  user's stored backend cookie **and folds rotated cookies back into the session**
  — so the backend's silent token refresh (§3.4) keeps working through the proxy.
- It joins the **shared Docker network** (`airplateserver_default`, marked
  `external: true`) so it can DNS-resolve backend services by name:
  - `scanner-sse:3010` — the SSE firehose (Fleet/Sensors live view)
  - `backfill:4400` — the Organizations backfill service
  - `api:3020` — the OpenAPI spec (Docs panel)
- It reads the **same MySQL** directly for some panels (`DB_HOST` /
  `airplate_dk_db_*`), mounts the host **Docker socket** (Observer), and can
  **SSH to deploy hosts** to drop OTA binaries (Sensors / AirPlates panels).

`dashboard-core` serves under a base path — `/monitoring` in dev, `/dash` in prod
(`ws.airplate.dk/dash`) — and is deployed as a prebuilt GHCR image
(`ghcr.io/airplate-aps/dashboard-core`).

---

## 6. How the three fit together (the relationships that matter)

1. **One backend, two frontends.** Both `app` and `dashboard-core` are clients of
   `AirPlateServer`'s REST API. `app` is the customer view (scoped to their org);
   `dashboard-core` is the god-view (all orgs, staff-gated).

2. **Shared MySQL is the join point.** `Organizations` / `Users` /
   `Users_Organizations` in MySQL are the source of truth both frontends read
   (directly or through the API). `organization_id` is the tenancy boundary
   everywhere.

3. **Two different auth systems that meet at the backend.**
   - `app` → **Firebase Auth** → Firebase cookie → backend verifies with
     `firebase-admin`.
   - `dashboard-core` → **its own JWT+TOTP session** → but for backend calls it
     **replays the user's backend cookie** via `backendFetch`, so it rides the
     same Firebase-cookie machinery. Staff identity is decided by the
     `@airplate.dk` email domain.

4. **Real-time is shared infrastructure, different doors.** The customer map uses
   the public **WebSocket** (`/scanning`); the staff fleet view uses the internal
   **SSE firehose** (`scanner-sse`), proxied so it never leaves the private
   network unauthenticated.

5. **`dashboard-core` lives *inside* the backend's world.** It's checked out as a
   submodule of `AirPlateServer`, joins its Docker network, reads its MySQL,
   proxies its services, and even renders its OpenAPI docs. `app` is more
   arm's-length — a static bundle on Simply.com hosting that only speaks HTTP/WS
   to the public endpoints.

6. **Data flow, end to end:**
   `scanner → airplate-ingest (UDP/TCP) → InfluxDB (raw) → intrusion-detector /
   flight-session-svc → MySQL (+ Postgres mirror) → api (REST) / websocket (WSS) /
   scanner-sse (SSE) → app (customers) & dashboard-core (staff)`, with
   `alert-manager` + notification services branching off to push/email/WhatsApp.

---

## 7. Environments & domains cheat-sheet

| Thing | Dev | Prod |
|-------|-----|------|
| Backend REST/WSS | `development.airplate.dk:5443` (Caddy/Traefik front) | `ws.airplate.dk:5443` |
| Customer app | `dev.airplate.dk` | `app.airplate.dk` (+ `fr.`, `mydefence.`) |
| Staff dashboard | `development.airplate.dk/monitoring` | `ws.airplate.dk/dash` |
| MySQL | `mysql65.unoeuro.com` (`airplate_dk_db_development`) | same host, `..._production` (+ self-hosted fallback) |
| InfluxDB | `:8086` (org `airplate`, bucket `droneid`) | same |
| Firebase project | `airplate-4062f` | `airplate-4062f` |

---

## 8. Mini-glossary

- **Organization** — a customer/tenant. The tenancy key (`organization_id`) on
  nearly every table and request.
- **Scanner / AirPlate / Sensor** — the physical detection hardware; identified by
  **IMEI**. "AirPlate" is also the product name.
- **Detection / scanned device** — one observation of a drone or its controller.
- **Flight log / flight session** — detections grouped into one drone flight.
- **UAS ID / Operator ID** — Remote ID identifiers for the drone and its pilot.
- **Alarm zone / geo-zone** — a polygon; a drone inside it triggers an intrusion.
- **Approved drone** — a serial the org has allow-listed (won't alarm).
- **Argos** — MyDefence's RF-geolocation hardware, reached over VPN.
- **OcuSync** — DJI's proprietary drone radio protocol.
- **Remote ID / FAA RID** — the broadcast standard drones use to announce identity;
  the FAA lookup maps a serial → manufacturer/model.
- **EOS** — the environment/telemetry timeseries subsystem (own Postgres + NATS).
- **Panel** — a self-contained feature module plugged into `dashboard-core`.
- **OTA** — over-the-air firmware update pushed to field devices.

---

*Generated from a read-through of the three repos in `praktik/`. If a specific
endpoint or service is unclear, the fastest ground truth is: backend routes in
`AirPlateServer/main/api/domains/*/index.ts`, the client wrappers in
`app/src/APIClient.ts`, and each panel's `panel.config.ts` in
`dashboard-core/panels/*`.*
