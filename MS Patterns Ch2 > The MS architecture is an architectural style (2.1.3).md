![[2.1.3.svg]]

---

### monolithic architecture
- structures the "implementation view" as a single component

### microservice architecture
- **(implementation view) multiple executables**
  
- **(logical view ) each service has its own architecture**
	- is typically a hexagonal architecture

- collection of loosely coupled independently deployable services
	- **the services in this architecture correspond to business capabilities**

- using interprocess communication mechanisms (REST/async messaging)
  
- key constraints
	- **services are loosely coupled**
	- restrictions on how the services collaborate

### what is a service (in MS)

- standalone, independently deployable software component
- API that provides its clients access
  
- two types of operations
	- commands
		- createOrder()
	- queries
		- findOrderById()

- a service also publishes events
	- OrderCreated

- API also encapsulates
	- a developer can’t write code that bypasses its API

- each service
	- **could has its own architecture and technology stack**

### loose coupling

- all interaction happens via its API (encapsulates impl details)
- **allows implementation to change without impacting clients**
- **making persistent data like "private fields of a class"**

- no database sharing
	- **also should not hold DB locks that blocks another service**
	- **(side effect) now having to maintain data consistency**

### role of shared libraries

- **devs often package functionalities in a lib (to reuse by multiple applications)**
- example
	- multiple services need to update "Order" biz object
	- pack this func as a library
	- **might look like a good way, have to ensure don’t accidentally introduce coupling**
	- consequences
		- **any change to the "Order" business object would require rebuild/redeploy of those services**

	- solution: only build libs for functionalities that's unlikely to change
		- example: a money lib

### size of a service

- services in MS aren't necessarily small
- well-designed service
	- **capable of being developed by a small team (also with minimal collaboration with other teams)**
	- if you constantly having to change a service (because of the underlying dependencies from other S)
		- sign it's not loosely coupled
		- **might be a distributed monolith**

