### Overview:
This document outlines the purpose and rough sketch for implementation for the webapp and app called "Netværket".
All technical documents will be written in english, but all UI and UX in the program will be in Danish, maybe english in a later stage. But the MVP/V1 will only include Danish userfacing strings.


### Purpose:
The core of this application can be divided in two. 
1. People who would like to find events to volunteer at in communities that they not necessarily are part of in their day to day, called **Group A** from now on.
2. Communities or event managers who need volunteers to help out with their events, called 
   **Group B** from now on.

### User flow:
The flow can also be described from both perspectives:
- Group A:
	- A person has time to spare, wants to meet new people or many more reasons to would like to help smaller commuties with their events. 
	  They create a profile with the following:
		- Basic information(Name, picture?)
		- Area they would be able volunteer in.
		- Qualifications (Types of driver licenses, profesion, cerficates, etc).
		- Ability to handle physical demanding work.
		- Contact information?(To be determined if the app should include that or just email/phone numbers)
- Group B:
	- A communty or event manager needs volunteers to host their event. 
	  They buy access to be able to create an event page and list their needs.
	  An event page should contain the following:
		- Name.
		- Type.
		- Location.
		- Open positions (Needs better wording - Open positions sounds too much like a paid job).
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
- Backend:
	- Written in C#
	- Incorporates Identity and Entity Frameworks for authentication and object management.
- Frontend - To be determined:
	-  Option 1: A Blazor frontend for MVP/V1 and later add a React Native frontend for cross platform apps.
	- Option 2: Straight to a React Native frontend that will handle both the webapp and native apps.
- Database:
	- A SQL database that for development purposes run from a local docker container and later will move to a hosted solution. Host is yet to be determined.