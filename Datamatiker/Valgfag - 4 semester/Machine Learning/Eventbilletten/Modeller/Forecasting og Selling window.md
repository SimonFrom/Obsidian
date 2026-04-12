## Mål:
- Output i form af kurve der viser et bud på hvornår de forskellige typer af events kan forvente at sælge og i hvilket omfang.
- Et bud på om man kan forvente at eventet vil blive udsolgt.

### Krav:
- Events og arrangører skal have angivet typer. Disse behøver ikke være 100% korrekte i denne omgang, men skal være konsistente og konsekvente.
- Der skal også sorteres nogle arrangører fra, Langesø MTB f.eks da de sælger billetter per behov og ikke per event. Disse arrangører skal identificeres og behandles.


### Klasse:
**Event:**
- `start` — event date, the target reference point for all time-based features
- `sellStart` / `sellEnd` — defines the sales window length
- `maxAmount` — total capacity, needed to calculate sell-through rate
- `tickets[].amount` — available inventory per ticket type
- `tickets[].price` — price point
- `locationCity` / `locationPostal` — venue location
- `categories[]` — seating zones and their capacities
- `seating.active` — whether it's a seated event
- `eventType` — SINGLE vs recurring
- `createdAt` — when the event was created (distance from creation to sell start)
- `showCountdown` — signals urgency marketing is active

**Transaction:**

- `createdAt` — exact purchase timestamp, core feature for building velocity curves
- `tickets[].amount` — tickets per order
- `realPrice` — actual revenue per order
- `status` — only COMPLETE orders should be used
- `salesChannelId` — web vs POS vs internal, affects sales pattern
- `ipLookup.lookup.city` / `.region` — buyer origin for geographic features
- `appliedCoupons` — whether discounts affected purchase timing
- `appliedDiscountGroups` — same

**Derived features you'd compute:**

- Days from sellStart to purchase (purchase timing)
- Days from purchase to event start (urgency signal)
- Sales velocity at day intervals (e.g. tickets sold per day)
- Cumulative sell-through % at each point in time
- Sales window length in days
- Capacity utilization rate