I started exploring this topic because I found an urge to build my microservicess in a logical way, and use hexagonal architecture for it. This led me to studying ddd as well.

1. Quick context on DDD [REDDIT] (https://www.reddit.com/r/programming/comments/196jwlb/understanding_domaindriven_design_part_1/)
	- One thing I want to add is that DDD is actually a reaction to another mainstream software design philosophy in ~2000 era.
	
	At the time there was a push for generic software design that worked across multiple industries. SAP, some IBM business process management software, generic ERP, “Workflow app” or even generic e-commerce.
	
	At the time the idea is that you should design a software in which so general that work in every company. So you ended up with things like “document”, “resource” or “workflow”.

	Then BA/Implementer/Consult come to general company. They look at business process.
	
	They said “oh your MBP2021 is just another resource with X plugin in our system”.
	
	They said “your [fancy name] flow is just like our workflow with extra steps. And will help you with those extra steps.”.
	
	They said “Our E-commerce is so powerful you can use in any company. You sell service? You can view service in our e-commerce system (which designed for physical product) as a virtual product with infinite stock. We will add isCutStockAfterSales configuration here”.
	
	And that how language drift usually happened. It is not just clumsiness or incompetence. It is usually come from conscious effort to make system generic.
	
	At that era, designing a general system such as ERP that work for every industry, E-Commerce that work for every organization, Workflow management that work for every single business flow imaginable, Accounting system that work for every country, are wet dream of software architects. Oracle, IBM, Microsoft and every big players are all aiming for that. (And I bet this is still the case for many organization and architect).
	
	That is when Eric Evans said that stop..... This aim for generality is not always something we all should strive for.
	
	What many people don’t understand about DDD is that when you committed to DDD you should not even aim for have that level of generality in the system anymore. It is against core philosophy of DDD.
	
	Many people try to have apply both generic system design philosophy, ruthless DRY and abstract as much as possible, with DDD and they will fail. Yes, now you need to have a separate entity or aggregrate for every business workflow with all exact term. You can't be a smartass and invented "ManagedWorkflow" base class which generalized many duplication you see without having this term in domain language anymore.
	
	Wether that is a good idea or not, I will leave it to you to decide. There is a clear upside and some downside.
	
	(In practice, if the code duplication is true duplication, usually domain will have term for it already or you can introduce it to domain expert easily. Fake duplication and [premature abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction) is a common problem.)
	
	And we can all see that those generic workflow/erp always come with caveat and weird limitation in organization context. DDD avoid that completely by having less generic system but design according to exact domain so any limitation in well-DDD system would likely to be something that domain expert understand, not a weird one.


2. Eric Evans - Domain Driven Design Tackling Complexity in the Heart of Software 
	   TODO ///READ SOMEDAY
3. How i first used Domain-Driven Design [ARTICLE](https://hackernoon.com/how-i-first-used-domain-driven-design-652814794567) 
   Domain Driven Design main objectives:
	- Ubiquitous Language(Powszechny język)
	- Domains and bounded context
		- Domain is the problem and Bounded context is the solution, Bounded context is often understood as a package (module) in Java's applications
	- Aggregates, Entities, Value Objects
		- Value Objects, Immutable classes, very good for validation, because we can validate only in the constructor
		- Entities are domain classes that are defined by unique identifier. They contain any business logic related specifically to that entity
		- Aggregates are logical grouping of entities. That make sense to be saved under an all or none(transactional) scenario
		- Repositories are classes used to save and retrieve aggregates.
4. WJUG #292 - Krzysztof Ślusarski "Porty, adaptery, CQRS, Event Sourcing, DDD… w Springu? [YOUTUBE](https://www.youtube.com/watch?v=Da42_gVqDKE)
	- Strong typing is helpful on big applications when debugging and seeing the FirstName Value on a stack instead of String i.e Also it is very helpfull
	- Example Value Objects has these annotations
		@Getter
		@Setter
		@NoArgsConstructor(access = AccessLevel.Protected, force = true)
		@RequiredArgsConstructor(staticName = "of)
		@EqualsAndHashCode(sometimes i saw here of="id)
		public class EmployeeId implements Serializable
	- Important idea to implement mock JPARepository
	- CQRS main motivations are:
		- We can use different models for persisting and different model for retrieving(i.e we don't want to show when retrieving some fields)
		- When persisting some entity we have a lot of validation and when retrieving we do not need that
		- When scaling up our application we can start making this calls asynchronous
	- From 01:00:00 things get more complicated, and i was not able to note everything.
	- CQS (Command Query Separation)
	- CommandBus
	- CommanHandler
	- Transactions
	- Event Sourcing
	- **Most important rule while creating software: YAGNI (You aren't gonna need it)**
	- Know the cons of technics before you use it
	- Use encapsulation whenever you can
	- Small modules with nice API and with proper tests are always better than big ones
	- @RepositoryRestResource its a annotation in Spring that can be quickly used as a CRUD controller for our entity



	- .................. TO BE CONTINUED
5. Gentle introduction to DDD [ARTICLE](https://blog.thelonearchitect.com/a-gentle-introduction-to-domain-driven-design-dc7cc169b1d)
	- **Vlad Khononov** - Leaning Domain Driven Design - BOOK RECOMENDATION
	- Active Records - An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data.
		- It knows how to persist itself to the database
		- includeds methods for quering and manipulating the data
		- it is responsible for its own CRUD
		- Active Record vs PersistenceDTO, Active record is not good fit for Hexagon because it violates the separation of concerns principle. With persistenceDTO for persisting to the db is responsible some other Repository class, and for Active record. So Active repoository interesting thing, good to know but won't be verty helpgull
		- There is two ways to dealing with additional logic related to entity, i.e User can have an email send to it.It can be synchonous or asynchronous. (Event Driven Design)
		- Synchronous is not very scalable. Harder to maintain when system grows.
		- Transactional Boundaries - it marks the start and end of transaction. It draws a circle around a set of actions and says. "All these actions should be treated as one big step". In other words it encapsulates elements(row in SQL or document in NoSQL) that must be consistently updated. Either transaction succed or all the steps rollback and go back to previous step. Atomicity - character A in ACID -> good to know
		- Aggregates - the role of aggregate is to encapsulate entities that must be treated as single unit. When we have User and Group we want to use Aggregate, and always use the Aggregate to operate on the group, WE CANT CHANGE THE GROUP ELSEWHERE.
		- Aggregate Root - each Aggregate has it Root, whose responsibility is to ensure the integrity of the aggregate. I.e for the above Aggregate root is Group
		- Domain Model - encapsulate the behaviour inside classes with precise names and use case that represents the bussiness. (We no longer use setters, but rather descriptive names for our actions)
		- Bounded Context, quite hard to explain but within bounded context bussines can define ubuquitous language completly different, User will be something different for Analitics BoundedContext and something different for Commerce Bounded Context. It should be as big as possible to avoid coupling, interactions between Bounded Contexts
		- Domain and Domain Model are NOT THE SAME (6 blind men and an elephant story)
		- Subdomain - Inside the large Bounded Context there are smaller subdomains. i.e in Commerce: Authentication, Inventory will be 2 subdomains.
			- Core - main subdomains, something different that we are making money from. Our creme de la creme
			- Supporting - support our business, but dont do no provide any advantage. Solve obvious problems and not very complex
			- Generic - Very obvious problems that was already being solved, i.e by third party library. Every our competitor resolves it the same way
6. Learning Domain Driven Design - Vlad Khononov
	- DDD methodology can be divided to:
		- strategic aspect - "what" and "why". What we are building and why we are building.
		- tactical - "how" we can build it ^66c156
	1. Analyzing business domain:
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
	6. Tacling Complex Business Logic
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
		- 








1. Confitura # 2022 - Jakub Nabrdalik - Things that work for me so well I cannot believe you are not using it [YOUTUBE](https://www.youtube.com/watch?v=5Er7juSAMXI)
   - "The feature already is here, it's just not evenly distributed" M.Wilson
   - While faced with instructions from business for implementing some new feature you need to ask question if this feature will be worth implementing, will it be used? or maybe it would be cheaper just do it manually
   - Refine requirements, very important. All written down in paper
   - As an architect it's very important to plan and foresee how the app will fail, how it should fail? very important
   - SLA - (service layer agreement) - document that outlines a commitment between a service provider and a client. including details of the service, the standards the provider must adhere to
   - FoC - (frequency of change)
   - Spock as a very powerfull testing tool
   - When we don't know what to do next. We need to implement Baby steps with TDD
   - Another possibility is a backward programming, when we start from last line of our feature to implement
   - Learning by chaos - printing whatever info we have, seeking, lurking etc
   - Best practises in coding:
	   - Fuckaround for awhile, explore possible solutions
	   - implement it
	   - delete it
	   - get closer, repeat
	   - do not be afraid of wasting time.
   - Logs should tell everything. DEBUGGER will help you but only locally.
2. The Identifier Type Pattern in DDD [ARTICLE](# Domain-Driven Design: The Identifier Type Pattern)
	   - Primitive Identifier vs Type identifier
	   - with Type identifier we can avoid "Primitive obsession code smell" that means avoiding the bug that we can have by mixing the order of the params
	   - it allows for overloading method. With method like findByOrder(string orderId, string customerId, string type), when we want to make method findByOrder(String orderId) that means that we cannot make another overloading findByOrder(String customerId) because they're of the same types
	   - it makes logic dependencies physiacal. It means if in Order we have customerId and in Customer we have the same information that if we change it in one place and won't change in the second than with Type identifier compiler will throw errors. The dependency is also physicall not only logical!!
	   - with type identifgier we can validate our Id
	   - Conclusion is that it;s good to use type identifier identity in our Entity
1. When and where to determine the ID of an entity [ARTICLE](https://matthiasnoback.nl/2018/05/when-and-where-to-determine-the-id-of-an-entity/)
	   -interesting design is to pass to the value object the id we want to declare. Because enity should not be aware of what id should be next in order. so we can implement in repository layer method nextIdentity() and from there take the next value which can be designed different for each storage system
2. Domain Driven Design - Q&A Sławomir Sobótka [YOUTUBE](https://www.youtube.com/watch?v=do-xqIbKZ_8)
   - How to tell the boundaries of aggregates
	   - How can the model be modelled
		   - "Being": 
			   - WHAT is the structure
			   - modelling what can into fall into trap of modelling on to data structures. i.e the order will cost money (float) and will have address (string)
			   - Modelling the data-base first. (Not optimal approach)"Zwykle w takich firmach gdzie jest święto bazy, rządzi kasta DB nazi"
		   - "Behaving"
			   - How something behaves,
			   - how the model react on signals
			   - What is changing
			   - Why is changing
			   - Under what circumstance it is changing
			   - Who changes it
			   - How often it is changed
			   - What are the consequences of change
		   - "Becomming"
			   - What is changed into
   - How to design the restfull API
   - What is the most important for modularity
   - why business doesn't talk to us
   - How to be a partner and now a worker
1. 



2. Problemy wynikające z modularyzacji - Łukasz Szydło
	--TODOoOOOOOOOOO
1. Architektoniczne antywzorce czyli na co chorują aplikacje - Łukasz Szydło


4. https://www.bottega.com.pl/pdf/materialy/ddd/ddd1.pdf
5. https://matthiasnoback.nl/2017/08/layers-ports-and-adapters-part-2-layers/
6. https://odrotbohm.de/2020/03/Implementing-DDD-Building-Blocks-in-Java/