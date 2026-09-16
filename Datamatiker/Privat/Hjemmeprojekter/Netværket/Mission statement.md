### Overview:
This document outlines the purpose and rough sketch for implementation for the webapp and app called "Netværket".
All technical documents will be written in english, but all UI and UX in the program will be in Danish, maybe english in a later stage. But the MVP/V1 will only include Danish userfacing strings.


### Purpose:
The core of this application can be divided in two. 
1. People who would like to find events to volunteer at in communities that they not necessarily are part of in their day to day, called **Group A** from now on.
2. Communities or event managers who need volunteers to help out with their events, called 
   **Group B** from now on.

### Trust & Safety:
- After an event, Group A and Group B should be able to rate each other. This also covers cancellations/no-shows (e.g. a Group A member not showing up, or Group B cancelling an event) rather than a separate cancellation policy, the app is only meant as a mediator between A and B, not an enforcer.
- There should be a way to report to developers or block another user.

### Privacy:
- To Be Determined: The app handles personal data (name, photo, location, qualifications, contact info) that becomes visible to Group B once a Group A member selects an event. This must comply with GDPR.

### Out of scope for MVP:
- Verification of Group B organizers (manual approval vs. self-service). TBD, not required for V1.
- In-app payments/reimbursement between Group A and Group B.
- Advanced in-app chat beyond basic contact/messaging on event match.
- MitID verification for Group A.

### Success metric:
- A concrete success metric for V1 (e.g. number of matched events, active users) is TBD.

### User flow:
The flow can also be described from both perspectives:
- Group A:
	- A person has time to spare, wants to meet new people or many more reasons to would like to help smaller commuties with their events.
	  They discover events via search/filter (location, category, date). 
	  They create a profile with the following:
		- Basic information(Name, picture?)
		- Area they would be able volunteer in.
		- Qualifications (Types of driver licenses, profesion, cerficates, etc).
		- Ability to handle physical demanding work.
		- Contact information?(To be determined if the app should include that or just email/phone numbers)
- Group B:
	- A communty or event manager needs volunteers to host their event. 
	  They buy access to be able to create an event page and list their needs. (Pricing model — per-event fee vs. subscription — is TBD.)
	  An event page should contain the following:
		- Name.
		- Type.
		- Location.
		- Helper roles.
		- Any compensation that Group A would recieve (food, drinks, accomendation, transport cost, etc).
		- Qualifications needed.
	- When a Group A member finds an event they would like to participate in, they select it the UI and Group B admin gets an notification with either contact info or the ability to message them directly in the app. 

It's important to note that it's **always** Group A that finds the events they want to participate in, Group B shouldn't be able to contact them without their consent or even see their profile before Group A selects the event.
Group A should also **never** have any costs involved with their use of the application, only transport costs to and from the event, which Group B may reimburse on their own will.


### Tech stack:
- Architecture:
	- Client/Server for seperation between front and backend.
	- Service and repository pattern.
	- SOLID principle.
	- DRY principle. Reuse or ensure polymorphism for classes and methods.
- Backend:
	- Written in C#
	- Incorporates Identity and Entity Frameworks for authentication and object management.
- Frontend:
	- Leaning React Native from the start, handling both the webapp and native apps. Lives in a separate repo from the backend. Not fully final yet.
- Database:
	- A SQL database that for development purposes run from a local docker container and later will move to a hosted solution. Host is yet to be determined.
- File storage:
	- Images (profile pictures, event photos) will be stored in blob storage (e.g. Azure Blob Storage), not in the SQL database. The database only stores references/URLs to the files.