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
- Role: `VolunteerA` | `OrganizerB` (open question: one `Role` enum, or fully separate account types from the start?)
- CreatedAt

**VolunteerProfile** (Group A, 1:1 with User)
- UserId
- Name
- ProfilePictureUrl (points to blob storage, see [[Mission statement]] Tech stack)
- Area / Location
- Qualifications: list of `Qualification`
- CanDoPhysicalWork: bool
- ContactInfo (nullable, still open whether this is collected at all, see Mission statement)

**OrganizerProfile** (Group B, 1:1 with User)
- UserId
- OrganizationName
- Description
- VerificationStatus (open: verification itself is out of scope for MVP, but the field may still need to exist as a placeholder)
- ContactInfo

**Qualification**
- Id
- Name
- Category: enum, e.g. `DriverLicense`, `Certificate`, `Profession`

**Event**
- Id
- OrganizerId (→ OrganizerProfile)
- Name
- Type / Category
- Location
- DateTime
- Compensation (free text or structured? open question)
- QualificationsNeeded: list of `Qualification`
- Status: `Draft` | `Published` | `Cancelled` | `Completed`
- CreatedAt

**HelperRole** (the renamed "Open positions", 1:N under Event)
- Id
- EventId
- Title
- Description (nullable)
- SlotsAvailable
- QualificationsRequired (nullable, overrides/adds to Event-level qualifications?)

**EventSelection** (a Group A member selecting into an event/role, name TBD)
- Id
- EventId
- HelperRoleId (nullable if roles aren't granular yet)
- VolunteerId
- Status: `Pending` | `Accepted` | `Declined`
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

### Open questions:
- Single `User` table with a `Role` field vs. fully separate `Volunteer`/`Organizer` account models. Affects Identity setup and how much shared logic (e.g. login) can be reused.
- Auth strategy for the API: leaning JWT, since React Native (a separate client repo, not server-rendered) needs a token-based approach rather than cookies.
- Is `EventSelection` a hard commitment or more like an "interest" that Group B still has to accept? Mission statement doesn't specify whether Group B can decline a volunteer.
- Should `HelperRole` be mandatory (every event must define roles/slots) or can an event just have a single generic ask?
- How are `Qualification` values managed: a fixed predefined list, or can Group B/Group A submit free-text ones?
- Where does GDPR data-deletion (right to be forgotten) hook into this schema: cascade deletes, soft deletes, or anonymization?
- Multi-tenancy: can one `OrganizerProfile` have multiple admin users, or is it strictly one User : one OrganizerProfile?
