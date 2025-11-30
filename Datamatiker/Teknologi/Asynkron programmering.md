[Async programming](https://medium.com/@leonardomartins_27620/c-asynchronous-programming-tasks-threads-and-async-await-aa28c7e65479)


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


### Nøglepunkter i async/await mønster:
1. **Asynkrone metoder** kan vente med at udføre deres indhold ved brug af *await* keywordet. Starter på main thread indtil man i metoden støder på await.
2. **Await** bruges til at vente asynkront på at en task færdiggøres. Den blokerer ikke main thread.
3. **Asynkrone metoder** kan returnere Task, Task<T> eller void.