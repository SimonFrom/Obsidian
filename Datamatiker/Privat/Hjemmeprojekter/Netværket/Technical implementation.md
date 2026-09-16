### Overview:
This document is a rough technical draft for "Netværket", following on from the [[Mission statement]]. Nothing here is final: folder structure and schemas are sketches to think through the shape of the system, not implementation yet.

### Folder structure (draft):
Backend and frontend live in separate repos. Frontend is leaning React Native from the start (webapp + native apps from one codebase).

Backend repo:
```
Netvaerket.Backend/
  src/
    Netvaerket.Api/                # ASP.NET Core Web API - controllers, Program.cs, DI setup
    Netvaerket.Application/        # Services, DTOs, interfaces - business logic (service layer)
    Netvaerket.Domain/             # Entities, enums, domain rules - no dependencies on other layers
    Netvaerket.Infrastructure/     # EF Core DbContext, repositories, blob storage client, Identity setup
  tests/
    Netvaerket.UnitTests/
    Netvaerket.IntegrationTests/
  docs/
  docker-compose.yml              # Local SQL container etc.
```

Client repo:
```
Netvaerket.Client/
  src/
    screens/                      # Or pages/, depending on navigation library
    components/
    services/                     # API client, auth token handling
    navigation/
  assets/
  tests/
```
Open question: exact React Native tooling (Expo vs. bare React Native CLI) not decided yet.

### Object schemas (draft):
Rough shape only. Types, nullability, and exact fields are not settled.

**User** (Identity-backed)
- Id
- Email
- Role: `Volunteer` | `Organizer` 
- CreatedAt

**VolunteerProfile** (Group A, 1:1 with User)
- UserId
- Name
- ProfilePictureUrl (points to blob storage)
- Area / Location
- AvailableRadius
- Qualifications: list of `Qualification`
- CanDoPhysicalWork: bool
- ContactInfo

**OrganizationProfile** (Group B)
- Id
- OrganizationName
- Description
- VerificationStatus (nullable for v1)
- ContactInfo
- WebsiteURL
- LogoURL
- Multiple Users can administer one OrganizerProfile — see `OrganizerAdmin` below. Not 1:1 with User.

**OrganizerAdmin** (join table, OrganizerProfile ↔ User)
- OrganizationProfileId
- UserId
- CreatedAt

**Qualification**
- Id
- Name (free text, normalized on submit — e.g. trimmed, capitalized)
- Category: enum, e.g. `DriverLicense`, `Certificate`, `Profession`
- Existing entries are surfaced as autocomplete suggestions when a user adds a new one, to nudge convergence on shared wording over time rather than enforcing a fixed list upfront.
- Exception: `DriverLicense` doesn't need free-text normalization since Danish driving license categories are a fixed, legally standardized list — this can be a real enum (`DriverLicenseCategory`) instead of free text:
	- `AM_Lille` — lille knallert, 30 km/t
	- `AM_Stor` — stor knallert, 45 km/t
	- `A1` — let motorcykel, op til 125 cm³
	- `A2` — mellemstor motorcykel, op til 35 kW
	- `A` — motorcykel, ingen effektbegrænsning
	- `B` — personbil, op til 3.500 kg
	- `B96` — personbil med tungere trailer end almindelig B tillader
	- `BE` — personbil med tungt påhængskøretøj/trailer
	- `C1` — lastbil, 3.500–7.500 kg
	- `C1E` — C1 med anhænger
	- `C` — lastbil, over 3.500 kg
	- `CE` — lastbil med anhænger (sættevogn)
	- `D1` — minibus, 9–16 passagerer
	- `D1E` — D1 med anhænger
	- `D` — bus, over 16 passagerer
	- `DE` — bus med anhænger
	- `T` — traktor/motorredskab

**Event**
- Id
- OrganizationId (→ OrganizerProfile)
- Name
- Description
- Type / Category
- Location
- Start and End date - DateTime
- Compensation: string
- QualificationsNeeded: list of `HelperRole.Qualifications`
- Status: `Draft` | `Published` | `Cancelled` | `Completed`
- CreatedBy
- HelperRoles

**HelperRole** 
- Id
- EventId
- Title
- Description
- SlotsAvailable

**EventSelection** (a Group A member selecting into an event/role, name TBD)
- Id
- EventId
- HelperRoleId
- VolunteerId
- Status: `Pending` | `Contacted` | `Declined` — B is notified once A selects, then decides whether to reach out (`Contacted`) or not (`Declined`); this is B's only checkpoint, since B can't see or contact A before this point.
- CreatedAt

**Rating** (see [[Mission statement]] Trust & Safety)
- Id
- EventId
- FromUserId
- ToUserId
- Score
- Comment (nullable)
- CreatedAt

**Report**
- Id
- ReportingUserId
- ReportedUserId
- EventId (nullable)
- Reason
- Status
- CreatedAt

**Notification**
- Id
- UserId
- Type
- Payload
- Read: bool
- CreatedAt
- Open question: delivery mechanism (email/push/in-app-only) not decided, see Mission statement Tech stack.

### Decisions:
- **User model:** Single `User` table with a `Role` field (`Volunteer` | `Organizer`), shared Identity/login logic. Group A/B separation happens in program flow, not at the account-model level.
- **Auth:** JWT, confirmed for the token-based API/React Native setup.
- **EventSelection semantics:** Not a hard commitment either A or B can be forced into. A selects an event/role; B is then notified and decides whether to contact A or not — that decision (`Contacted` / `Declined`) is B's only real checkpoint, since B has no visibility into or access to A before this point.
- **HelperRole:** Mandatory. Every event defines at least one `HelperRole`, even for a single generic ask.
- **Qualification management:** Free text, normalized on submit (trim, capitalize, etc.) rather than a fixed predefined list — a fixed list would need full up-front coverage of every possible human qualification, which isn't feasible. As the qualification table fills up, existing entries feed autocomplete suggestions, nudging convergence toward shared wording over time instead of enforcing it upfront.
- **Multi-tenancy:** One `OrganizerProfile` can have multiple admin `User`s. Modeled as `OrganizerProfile` ↔ `User` via a join table (`OrganizerAdmin`), not a 1:1 relationship.

### Open questions:
- Where does GDPR data-deletion (right to be forgotten) hook into this schema: cascade deletes, soft deletes, or anonymization? Still to be determined.
- Contact flow: Right now I'm leaning towards having Group A contact Group B managers to volunteer. Not the other way around. This is in part to ease the work load on Group B manegers. This way, one person has to contact one person. The other way it would be, one person has to contact many persons.