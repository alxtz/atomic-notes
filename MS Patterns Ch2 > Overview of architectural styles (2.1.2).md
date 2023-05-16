![[2.1.2.svg]]

---

### **three-tier architecture (style)**

- **presentation layer**
	- code that implements the user interface or external APIs
	- **typically FE websites**

- **business logic layer**
	- contains business logic

- **persistence layer**
	- application's persistence storage
	- **typically databases**

- (drawbacks)
	- single presentation layer
		- **application's are likely to be invoked by multiple systems (FE/mobile/other servers)**

	- single persistence layer
		- **application's are likely to interact with multiple type of DBs**

	- **business logic layer to directly depend on persistence layer**
		- prevent you from testing business logic without DB (business logic are draft from the DB level)

### **well designed application**

- business logic
	- **typically defines data access methods (repositories/interfaces/types)**

- persistence layer
	- **implements repository interfaces (DAO classes)**

- dependencies are the reverse
	- **business logic doesn't depend on the storage layer;
		- instead storage layer should implement DAO classes based on what business logic needs**

### **hexagonal architectural style**

- alternative to layered architectural style
	- **places business logic in the center**

- instead of presentation layer
	- **had inbound adapters (handles requests from outside)**

- instead of persistence layer
	- **had outbound adapters (invokes by the business logic) (invokes external application/services)**

- **business logic doesn't have to strictly depend on the adapters (storage could become a replaceable concern)**

- business logic has ports
	- inbound ports
		- API exposed by the business logic

	- outbound ports
		- how business logic invokes external systems

- adapters
  
	- inbound adapters
		- MVC (API) controller
		- msg broker client (subscribes to messages)

	- outbound adapters
		- DAO (implements operations for accessing database)
		- event publishers

	- **multiple adapters (with the same direction) has chances to invoke the same inbound port**

- decouples business logic from the presentation
	- **the business logic doesn’t depend on either the presentation logic or the data access logic**

- fits the MS architecture more

