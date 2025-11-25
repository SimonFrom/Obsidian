Mønstre i async programmering:
- **Event-Based Asynchronous Pattern - EAP**
*Microsoft anbefaler ikke dette mønster til ny udvikling mere.*
	- Bruger eventhandler delegates
	- Typer nedarvet fra EventArg
	- Almindelig navngivning
		- ReadAsync, ReadCompleted, ReadAsyncCancel f.eks.
	
- **Asynchronous Progamming Model - APM**
*Microsoft anbefaler ikke dette mønster til ny udvikling mere.*
	- Bruger IAsyncResult interface
	- Asynkrone operationer kræver både en start og slut metode
		- BeginWrite og EndWrite f.eks.

- **Task-Based Asynchronous Pattern - TAP**
	- Bruger tasks og await
	-  