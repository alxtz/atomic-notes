![[2.1.1.svg]]

---

### essence of MS architecture
- to perform functional decomposition of an application
- how can we come up with the definition of those services?

### strategies for decomposing
- **services are biz concern oriented, not technical**
- using DDD to deal with specifically god classes

### what is MS architecture exactly
- **key idea: functional decomposition**

### what's the difference between the following
- **software architecture**
	- a lot of definitions
		- Len Bass
			- **the application's elements & their relations (defines how they interact)**
			- facilitates the division of labor and knowledge

- **architectural style**
	- Roy Fielding's definition
		- (ref: 3.8.1 Classification of Architectural Styles and Patterns)
		- **a set of principles & constraints, that could be mixed up to help building custom software architectures**
	- examples
		- **monolithic architecture style**
			- implementation view as a single (executable/deployable) component
		- **MS architecture style**
			- application as a set of loosely coupled services
		- **layered architecture style**
			- organizes software elements into layers
			- layer has a well-defined set of responsibilities
			- **a layer can only depend on either the layer immediately below it (if strict layering) or any of the layers below it**
- **tech stack/technical decision making**

### what is a software architecture
- software application's high-level structure
- **application's architecture is multidimensional**
	- **there are multiple ways to describe it**
- architecture determines the application's "x-ilities"

### software architecture views
- **the "4+1 view" of software architecture**
- software architecture, just like housing architectures, is multi-dimensional
	- **using multiple views to better portrait this**

- 4+1 view
	- logical view
		- classes/packages, and their ER relationships
	
	- implementation view
		- executable/deployable units, and how they compose
	
	- process view
		- how components interact at runtime

	- deployment view
		- how the processes are mapped to machines

	- **scenarios (the +1 view)**

- **interestingly, it doesn't describe the tech stack that much**

### requirements in an application

- **functional requirements**
	- **software architecture have little to do with this**

- non-functional requirements
	- its quality of service requirements