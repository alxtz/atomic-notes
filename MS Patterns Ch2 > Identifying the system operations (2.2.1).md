![[2.2.1.svg]]

---


### **how to define a MS architecture's (domain/object/services)**

- identify key requirements

### decomposite strategies 

- decompose by biz capabilities
- organize services around DDD sub-domains

### determine service's API

- **some service operations could be implemented by itself, while others need collaboration**

### obstacles

- **network latency**
- **sync communication reduces availability**
- **need to maintain data consistency**
- needing to deal with "god classes"

### identify operations

- start from the application's requirements (user stories/user scenarios)
- using "2-step process"
	- **like OO design process (from UML patterns)**
	- **using nouns from (BDD style) user stories**
	- **use verbs for system operations**
	- could also use "event storming"

- **a typical system operation**
	- **can create/update/delete domain objects**
	- **can create/delete relationships between those**

- steps
	- **identify (domain model) objects (high-level domain model)**
		- sketch a high-level model
			- much simpler than the actual implementation
			- defines the vocabulary & behaviors

		- **analyzing the nouns/talking to domain experts**
		
		- example: place an order
			- Given a consumer
				- And a restaurant
				- And a delivery address/time that can be served by that restaurant
				- And an order total that meets the restaurant's order minimum
			- When the consumer places an order for the restaurant
			- Then consumer's credit card is authorized
				- And an order is created in the PENDING_ACCEPTANCE state
				- And the order is associated with the consumer
				- And the order is associated with the restaurant
		- **nouns: Consumer, Order, Restaurant, and CreditCard**

		- example: accept an order
			- Given an order
				- that is in the PENDING_ACCEPTANCE state and a courier that is available to deliver the order

			- When a restaurant accepts an order with a promise to prepare by a particular time

			- Then the state of the order is changed to ACCEPTED
				- And the order's promiseByTime is updated to the promised time
				- And the courier is assigned to deliver the order
		- **nouns: Courier and Delivery**

	- define system operations
		- identify the requests the application should handle
			- **2 type of sys operations commands & queries**

		- **starting point -> analyze the verbs in user stories/scenarios**

			- Place Order story -> CreateOrder() operation
			- (key system commands)

		- specification of a command
			- **parameters**
				- **createOrder (consumer id, payment method, delivery address, delivery time, restaurant id, order line items)**

			- **returns**
				- orderId

			- **preconditions**
				- **mirrors the "GIVEN" statement in BDD**
				- consumer exists and can place orders
				- line items correspond to the restaurant’s menu items
				- delivery address and time can be serviced by the restaurant

			- **postconditions**
				- **mirrors the "THEN" statement in BDD**
				- consumer’s credit card was authorized for the order total
				- order was created in the PENDING_ACCEPTANCE state

		- **most of the architecturally relevant system operations are commands**
			- some queries, like "findAvailRestaurants()" might also be architecturally significant; as it touches geosearch, also being performance sensitive 
		- system operations defined here, each represents an architecturally significant scenario
		- **next step -> figure out what should be a service**

			- revisit: services should be organized around business concerns

### this really quite different then the "novice" way of designing the system

- what I used to do

	- **sometimes there isn't always a BDD style user story, so the BE even look at the frontend UI to determine what "field" do they need**
	- **once we've collected all the requirements, ppl started from drafting the DB schema; assuming it being the most relevant part of the system + it could be the most performant**
	- **domain/system objects directly reflects what DB tables do we have**
	- **(when things become complex/insufferable)
		- requirements from distinct scenarios (ex. HomeScreen/MF/BillPayments)
		- having to operate/query from the same table
		- various fields having to be introduced to both the table & domain object;
		- complexity of the domain object explodes, having to make much cross-team communications, no one know how BankingClients/Transactions exactly works**
		- examples
			- **Transactions**
			- **BankingClients**
			- (partial) Kyc

- takaways
	- even if we're not breaking things down to services yet, feels like the philosophies introduced in this chapter could help our day to day product development process 
	- **should reverse our system design process from considering DB schemas first; but working on drafting the capabilities/operations/domain objects**
	- this way of organizing services doesn't take technical limit into consideration;
	- but we'll face it eventually is it?

