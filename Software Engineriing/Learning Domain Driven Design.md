- DDD methodology can be divided to:
		- strategic aspect - "what" and "why". What we are building and why we are building.
		- tactical - "how" we can build it ^66c156

1. Analysing business domain:
	- Business domain - is a main field that company is making money form, like Starbucks -> coffee. Companies can have multiple domains, like Amazon -> online retail and cloud services.
	- Subdomain - fine-grained area of business activity. In example Starbucks is coffee + real estate rental + HR + manage finances
		- Core subdomain - main area, that's how company makes money. + Complexity (usually complex systems) + source of competitive advantage. It is constant evolution of the idea and the solutions. It is incrementally getting better
		- Generic - SOLVED PROBLEMS, repetitive like authorization (widely tested by others, no need to reinvent the wheel). It can be bought or adapted from other open-source solution
		- Supporting - something made for ourselves, it is quite easy to do it in-house. So the difference between generic and supporting is that generic might be complex but other companies has similar problems and solutions, and supporting might be easy but it need to be custom to our needs. When it comes to changes, it doesn't change much, it rather stays the same. It can't be bought, because even if its easy, no one really needs that specific code so we need to make it ourselves
		  ![[Pasted image 20240529205756.png]]
	  - Domain expert - represents the business, it can be someone with requirements or end user
2. Discovering Domain Knowledge
	- Business problem - set of challenges to  automate/optimise workflows/processes
	- Ubiquitous language - instead of line like domain experts, analytics, project owners, programmers where the message can be lost in translations we should use the same language for our domain business.
	- Model - model is a simplified representation of a thing that highlights some aspects while ignoring others. Example maps. It can be milion different types of maps of the same area. i.e Kraków. it can be road map, terrain map, tram map, river map. None of the map represents all the details. The same with Model we need the representation of a thing that suits our needs ^328415
	  **"All models are wrong, but some are useful"**
	- Tools for gathering domain knowledge 
		- Wiki as a glossary for definitions. Nouns. Best when glosary is shared effort. Everyone should update the wiki to best keep with the changing and evolving domain
		- Gherking use cases
		  Feature: Guess the word
			  Scenario: Maker starts a game
				When the Maker starts a game
				Then the Maker waits for a Breaker to join
			  Scenario: Breaker joins a game
				Given the Maker has started a game with the word "silky"
				When the Breaker joins the Maker's game
				Then the Breaker must guess a word with 5 characters
		  - For Ubiqutous language
			  - NO SYNONYMOUS WORDS
			  - NO AMBIGOUS WORDS
3. Managing Domain Complexity
	- There can be situation where it's hard to get ubiquitous language, i.e one e-commerce online shop can have different meaning for every other subdomain, ie 'Lead' will be different for sales team, and different for martketing team. - The solution might be Bounded Context. We are creating smaller ubiquitous language isntead of one big and it is bounded context. ie bounded context marketing which has 'Lead' as simple entity and Bounded Context Sales which has complex Entity 'Lead' So for maps the bounded context will be the adjective of an map. i.e terrain map the bounded context will be terrain. Therefore a model (map) is only usefulwithin the context of its specific purpose
	- Bounded COntext vs - Subdomain -- Subdomain comes from the business people which can show the exact boundaries of each subdomain. Whereas bounded context is designed by developers. It depends on the case but it can be i.e monolithic and contain several subdomain or it can be fine-grained and consists bounded context for all subdomains 
		- Subdomains are discover while bounded context are designed
	- Each bounded context should be implemented as separate service/project
	- Bounded context are physical boundaries while subdomains are logical boundaries
	- Bounded context should be managed b one team only, but one team can manage up to several bounded contexts
	- Does bounded context exists in real life? Indeed they do. I.e tomato in botanical terms is a fruit, while in culinary art it's a vegetable, these are different bounded context. In the taxation bounded context the tomato was fruit in 1883 but ten years later it was changed to a vegetable (to be properly taxated). Great quote - **in the bounded context of theatrical performances, the tomato is a feedback mechanism.**
4. Integrating bounded context
	- There is couple ways to implement bounded context
	- COOPERATION PATTERNS
	- Cooperation - it is when implementing couple bounded context there is stron communication and all the differences can be addressed quickly. In most cases these are bounded contexts managed by one team
	- Partnership - it's when different teams has strong communication and can address changes quickly and they both need each other to succeed. Not best solution for teams in different time zones.
	- Shared kernel - when we will use the same models in couple bounded context. In monorepo it can be separate catalog. in microservices it can be i.e external library. Should be implemented only when the cost of duplication is higher than cost of coordination
	- CUSTOMER-SUPPLIER PATTERNS
	- Customer-Supplier - both teams can succeed independently. There is always some inbalance of power
	- Conformist- Doesnt reallly care about customer needs. He defines contract, and the customer can take it or not. So the customer can conform for the upstream bounded context
	- Anti-corruption layer- still upstream has more control but the customer can tailor the upstream to its own needs. It can translate the upstream bounded context model into a model tailored to its own needs.
	- Open-Host-Service - supplier want to protect consumers, and it's decoupling the implementation from the public service and therefore it can be differently evolved.
	- SEPARATE WAYS PATTERNS
	- Communication issues- when the communication issues based on organization size or internal politics comes in we may want to resign from collabaoration with other bounded contexts
	- Generic subdomains- sometimes it makes more sense and its cheaper to implement something in each bounded context instead of exposng it as a service. I.e logging. There is no much sense in espoxing logging as a separate service when it can be done from each bounded context
5. Implementing Simple business logic
	- transaction script - organizes business logic by procedures where each procedure handles a single request from the presentation
	- example transsaction script:
		- addItem(String item)
		- setPriority()
		- list()
		- assign(String item)
	- Authors discussed bad implementation of transactions script(which is the easiest way of writing a software, which is not that easy.)
		- i.e implementing two. separa actions, can be tricky while we cannot guarantee the transaction (not all db are supporting this, and also sometimes we don't know what storage system will be used)
		- 1 solution is to try make everything indepotent. Like requesting a visits number and then passing the increaesed visit number as an param, which will always pass the correct value.
		- 2 solution: optimistic concurrency control. We will call the source for a visits number and will pass it as a param and will update the value ONLY if the value in db is the same as the passed one.
	  - Transactions script is being discussed as an anti pattern sometimes, it should not be used for core subdomain, It will quickly turn into big bull of mud when complexity grows.
	  - Active record- an object that wraps a row in a database table of view, encapsulates the database access, and adds domain logic on that data.
	  - These objects also implements the CRUD operations that means that they're always tight coupled with ORM of some sort.
	  - Accordingly as the transaction scripts it doesn't work well with core subdomain and should go rather in supporting subdomain.
	  - It is also know as a anemic domain model anti-pattern, but author would prefer not to treat it as an antipattern. Its only a tool. he says there is nothing wrong with using active records when logic is simple
6. Tackling Complex Business Logic
	- A domain model is an object model of the domain that incorporates both behaviour and data!
	- DDD's tactical patterns - Aggregates, Value Objects, Domain Events, Domain Services - are the building blocks of such an object model.
	- Because the logic behind the system we want to implement is already complex, we should avoid additional complexity in our domain models. It should be free of infrastructure code or technological concerns (no database conections, etc. --> It should be just POJOs)
	- We should aim for using Value Objects 
		 - no problems with wrong data (fail fast)
		- more options for validation, the validation is in the value objects
		- i.e we can gather some rules regarding specific value object and manipulate it easily like Height and we can implement methods for creating height based on metric system or imperial and even compare these two.
		- Good example of value object that is always created as new one is JAva String class. It;s always creating new instance, not updating the state of former object
		- As a rule of thumb we should try to use Value Objects whenever we can. It;s a good practice to use all of the pros of this approach. The Value Object should be used for Domains elements that describe properties of other objects.
	- Entities - opposite to value object. Entity require an explicit identification field to distinguish between the different instances of the entity. I.e person can't have only name property, it needs to have some identifier, because two persons with the same name are still different people.
	- Entities ARE NOT IMMUTABLE and are expected to change (we implement logic for entity to change)
	- Aggregate is an entity. But to be completly honest is so much moe than just entity. Aggregate responsibility as a pattern is to keep the data consistent
	- Consistency enforcement is a key characteristic of an aggregate. Aggregate draws the boundary between the aggregate and outer scope. To aggregate goes all the modification tham means that it works on keeping it consistent and in line with business rules. 
	- Only aggregate logic is allowed to modify state, everything external is allowed only to read the state
	- Aggregate is exposed as public interface, which is referred as an "command"
		 - It can be implemented in two ways:
		- the public interface can be implemented as an public plan method of the aggregate object
		- Command can be represented as a parameter object that encapsulates the input required for executin command
	- Aggregate encapsulate the rules that MUST be correct instantly
	- Value objects we compare based on values and aggregates based on its identity
	- It is crucial to ensure that the database we work with support the concurency management. When we want to do any action on the object in database we will check if the version after update is what we expect and only then commit the changes. Otherwise we will rollback and return error
	- Aggregate is a place of transactions boundary. Any changes made to the aggregate must be commited or rollbacked
	- The boundaries only occur individually. One aggregate per database transaction. If we need to use many aggregates within one database transaction than it's bad and we need to redesign our architecture
	- Hierarchy of Entities. We don't use Entities as individual pattern. Only as a part of Aggregate
	- Aggregate is named that way because it gathers or business entities and value objects that belongs to the same transaction boundary
	- Since all objects contained by and aggregate share the same transactional boundary the performance issue can erase when aggregate grows too large.
	- Aggregate is a place for strongly consistent data/logic. If something can be "eventually" consistent then it's out of aggregate boundary
	- in Domain Model we can have something like this: ![[Pasted image 20240604204352.png]] List of messages belongs directly to the ticket, that's why it a list of Messages, but list of products and users are not so closely related that's why they're referenced only by Id's
	- To decide whether entity belongs to aggregate, we should examine whether it contains any businness logic that can lead to an invalid state of our system.
	- Aggregate Root is like interface for executing any changes to the aggregate
	- Domain events is the second form of communicating aggregates with outer world. Domain events also belong to the aggregate and can be published from within the aggregate, and somwhere else maybe even in other microservice someone can subscribe to some of our events.
	- Domain service is used when some logic doesn't belong to particular aggregate, or belongs to more than 1 aggregate, when we create domain service. (i understand it's very similar to service class in layered architecture)
7.  Modeling the dimension of time
	   - Event sourcing as a way to add time dimension into our domain models, In example statuses of a ticket can be displayed as events and therefore we gat all of the information regarding the particular event in time
	   - For the event sourcing patterns, all changes to the object state should be represented and persisted as events
	   - Event store as a database for events
	   - Event sourced domain model which is explicitly stated that we are using event sourcing to represent changes in the lifecycles of the domain model's aggregates
	   - Event sourcing - advantages:
		   - time-travelling - domain events can be used to displaying the current state it can be also used to retrieve any past state of an aggregate. It's good for inspecting the flow, optimising the processes and so on. It also good for retro-active debugging, because we can revert the state of an aggregate to the state it was at the moment of the bug occurance
		   - Deep insight - which is crucial for core subdomain, we can always add another projections that will help to provide additional information about the domain.
		   - Audit log - domain events display great audit log of event that took place
		   - Advanced optimistic concurrency management - 
	   - Event sourcing disadvantages:
		   - High learning curve: big difference from the traditional way of managing data.
		   - Evolving the model - evolving the events in the future can be quite challenging. It's definitely much harder than editing a schema
		   - Architectural complexity - making the design more complicategod
	   - Performance issues - for most systems problems will arise after 10 000 events per aggregate. But statistically average aggregate contains only 100 events.
	   - Event store is append only database
8. Architectural patterns
	- Author talks about layered architecture, port and adapters and CQRS
	- Polyglot modelling - Since all databases are flawed in it's own ways. We always have to balance for scaling, consistency and supporting querying models. Alternative to finding perfect database is polyglot modelling which is implementing different databases for different data-related requirements
	- CQRS segregates responsibilities of the system's models. There are two types of models: the command(execution. model), and the query(read model)
	- Model segregation it's not that the i.e command model can't read any data. If that's the case maybe it's time to rethink your architecture. Commands in may cases should return some data, information about new state and so on which can help in further workflow without any need for invoking query for getting the data instead.
9. Communication patterns
	- Here the talk is about comunicating (See chapter 4 - COOPERATION PATTERNS)
	- Stateless model transformation:
		- synchronous
		- asynchronous
	- Stateful model transformation:
		- Aggregating incoming data (when bounded context want to aggregate incoming requests and process them in batch for optimisation reasons, or when unifying incoming requests)
	- When publishing events we can use Outbox pattern
	- I STOPPED HERE, BECAUSE THE TOPIC SEEMS TO ADVANCED FOR ME. I DON"T HAVE ANY IDEA WHAT THE AUTHOR IS TALKING ABOUT. HOPE TO COMEBACK LATER.
	- Saga pattern- which is connecting two different aggregates, maybe even from other bounded contexts. and help with managing the span of life of transactions
	- Process manager pattern 
10. Design Heuristics
	- Bounded context - how big? Is the bounded context equal to 1 microservice: --> We should not rate the bounded context by its size, rather try to know about domain maximum information to properly decide about the scope of each bounded context because further changes in requirements which span across multuple bounded context (with other teams) are very costly. It is better to start with wider scope and with time try to make bounded context smaller, when we have more information about domain and future needs (mostly applicable to core subdomains).
	- Business logic implementation pattern
	  ![[Pasted image 20240605210518.png]]
  
	- Architectural Patterns
	  ![[Pasted image 20240605210913.png]]
	-  Testing strategy:
	  ![[Pasted image 20240605211124.png]]
11. Evolving Design Decisions
    - In this chapter the author says a lot about migrating between different patterns used for modeling architecture but also about migrating different patterns for communication
	- There is also talk about keeping our tools sharp, like subdomains and bounded context, and with growth there is crucial to revisit initial design and adjust to our needs
	- Bounded context should be possible to solve big part of its problems ourselves when its getting more and more chatty with other bounded contexts its signal for us that or model is ineffective
	- Aggregates, when accumulate more and more data that is not needed to be strongly consistent then it;s best to extract business functionality to a dedicated aggregate
	- Continuosly check the different boundaries for signs of growth-driven complexity. Act to eliminate accidental complecities, and use domain driven design tools to manage the business domains essential complexity
12. Event storming
	- eventStorming is a low tech activity for a group of people to brainstorm and rapidly model a business process. Its a tool for sharing business domain knowledge
	- EventStorming has a scope that the participants want to explore
	- We want to know as much as possible as quick as possible
	- Key ingredients:
		- Modeling space (the bigger, the better)
		- Stick notes (lots of, with different colors)
		- markers
		- snacks
	- EventStorming Process
		- Step 1: Unstructured exploration
		  brainstorm of domain events (formulated in past tense)
		- Step 2: Timelines 
		  Next we organize all domain events in the order in which they occur in the business domain
		- Step 3: Pain points
		  Identify points in the process that require attention (bottlenecks, manual steps that require automation, missing documentation, missing domain knowledge)
		- Sterp 4: Pivotal events
		  Identify significant business events indicating a change in context or phase. "Pivotal events". For example "shoping cart initialized", "order initialized", "order shipped". Pivotal events are indicator of potential bounded contextx boundaries
		- Step 5: Commands
		  Commands describe what triggered the events or flow of events described earlier. Commands are formulated in imperative like: "Publish campaign", "Rollback transaction", "Submit order"
		- Step 6: Policies
		  Almost always there are commands which have no specific actor associated with them. During this step we look for policies that might execute those commands.
		  Automation Policy is a scenario in which a event triggers the execution of a command 
		- Step 7: Read models
		  a read model is the view of data within the domain that the actor uses to make a decision to execute a command. this can bee a system's screens, reports, notifications
		- Step 8: External systems
		  External system is something that we do not explore within the eventstorming session. It can execute commads, and be notified about events.
		- Step 9: Aggregates
		  In this steps the participants can start thinking about organizing related concepts in aggregates. Aggregate receive commands and produce events.
		- Step 10: Bounded Contexts
		  This step is about looking for aggregates that are clousely related funcitonalities or because they're coupled through policies 
13. Domain-Driven Design in the Real World
    - Nothing new in this chapter, just bunch of tips, which I don't know how to summarize
14. Microservices
    - 