# AirPlate — From Detection to UI (step by step)

> The life of a single drone detection: from the moment a scanner in the field
> hears a drone, to the moment it lights up on the customer's map in the `app`.
> Companion to `ARCHITECTURE.md`.

---

## The one-line version

```
Drone radio → Scanner → ingest → InfluxDB (raw) → detection processing
→ MySQL (stored) + enrichment → WebSocket / REST → app → Mapbox map
                                        └→ alerts (push / email / WhatsApp)
```

---

## The steps

### 1. A drone broadcasts

A drone (and its controller) emits radio signals — **Remote ID** beacons, DJI
**OcuSync**, or RF that the MyDefence **Argos** hardware geolocates. This includes
identity (UAS ID, operator ID, serial) and often position.

### 2. A scanner hears it

A physical **scanner / AirPlate** in the field (identified by its **IMEI**) picks
up the signal, decodes it into a structured detection, and streams it to the
backend over **UDP (7070) / TCP (8082)**. Argos data instead arrives through the
`mydefenceClient` VPN bridge.

### 3. Ingest receives the raw telemetry

The **`airplate-ingest`** service accepts the stream and writes the raw,
high-volume position/RF data into **InfluxDB** (org `airplate`, bucket `droneid`).
InfluxDB is the first landing zone because it's built for time-series firehoses.

### 4. Live fan-out to the map (the fast path)

For the *real-time* view, `airplate-ingest` also POSTs the detection (over the
internal `/publish` endpoint, shared-secret protected) to **`airplate-ws`**, which
**broadcasts it over WebSocket** to any connected client. This path is optimized
for latency — it does **not** wait for database writes.

### 5. Processing & storage (the durable path)

In parallel, the detection is processed and persisted:

- **`intrusion-detector`** checks the drone's position against the org's
  **alarm zones**. Inside a zone → it's an **intrusion**.
- **`flight-session-svc`** groups repeated detections of the same drone into a
  **flight session / flight log**.
- The result is written to **MySQL** (the relational source of truth), and mirrored
  into **PostgreSQL** for analytics.

### 6. Enrichment

The backend adds context the raw signal didn't carry:

- **FAA Remote ID lookup** turns a bare serial into a **manufacturer + model**
  (cached in-process on the API for speed).
- Cross-checks against the org's **approved-drones** allow-list (so a known-friendly
  drone doesn't raise an alarm).
- Geocoding / zone tags for where the track is.

### 7. Alerts branch off (if it matters)

If the detection is an intrusion (or matches a notification rule), the
**`alert-manager`** fans it out to the org's **notification contacts** through the
**mail / WhatsApp / signal / push** services. This happens independently of anyone
having the UI open.

### 8. The customer's app is already listening

When a customer opens the `app`, it authenticates via **Firebase Auth** and opens a
**WebSocket** to `${WSurl}scanning?organization_id=X`. Because it's scoped by
`organization_id`, the client only ever receives detections for **its own org**.

### 9. The message arrives in the browser

The WebSocket message from step 4 reaches the `app`. Its realtime handler
(`realtime/scanning.tsx`) parses it and updates the **Zustand store**
(`droneMapStore`) — the latest message per drone is kept for click/notification
handling.

### 10. It's drawn on the map

The handler creates or updates icons on the **Mapbox** map:

- a **drone** icon and its **operator** icon,
- a dashed **yellow line** connecting the two,
- a **toast** popup and a **bell** entry in the top nav announce the detection.

### 11. The customer clicks for detail (REST path)

Live WebSocket gives *position now*; history comes from **REST**. Clicking a drone
calls the backend's `coordinates*` endpoints (via `APIClient.ts`) to fetch and draw
the full **trails** — blue for the drone track, red for the operator track — plus
flight-log details, all served from MySQL/Influx and rendered by React Query.

---

## Two paths, summarized

| | Fast path (live) | Durable path (history/detail) |
|---|---|---|
| **Carries** | "a drone is here right now" | stored detections, tracks, flight logs, stats |
| **Route** | ingest → `airplate-ws` → **WebSocket** → app | ingest → InfluxDB/MySQL → **REST API** → app |
| **Trigger** | pushed automatically on every detection | pulled when the user opens a page / clicks a drone |
| **In the UI** | icon appears + moves, toast, bell | trails, intrusion log, dashboards, heatmaps |

Both paths are scoped by `organization_id`, so a customer only ever sees their own
airspace.

---

## Where the staff dashboard fits

`dashboard-core` watches the **same pipeline from the operations side**: instead of
the per-org `/scanning` WebSocket, it proxies the internal **`scanner-sse`**
firehose (all orgs at once) to show *scanner fleet health* — which sensors are
online and detecting — rather than individual customer drones.

---

*Ground truth for each step: `AirPlateServer/main/api/domains/coordinates` &
`/detections` (REST), `main/websocket` + `airplate-ws` (live push),
`app/src/realtime/scanning.tsx` (map drawing), `app/src/APIClient.ts` (REST calls).*
