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
6. Learning Domain Driven Design [[Learning Domain Driven Design]]
   - Notes are stored in separate file
7. Confitura # 2022 - Jakub Nabrdalik - Things that work for me so well I cannot believe you are not using it [YOUTUBE](https://www.youtube.com/watch?v=5Er7juSAMXI)
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
8. The Identifier Type Pattern in DDD [ARTICLE](# Domain-Driven Design: The Identifier Type Pattern)
	   - Primitive Identifier vs Type identifier
	   - with Type identifier we can avoid "Primitive obsession code smell" that means avoiding the bug that we can have by mixing the order of the params
	   - it allows for overloading method. With method like findByOrder(string orderId, string customerId, string type), when we want to make method findByOrder(String orderId) that means that we cannot make another overloading findByOrder(String customerId) because they're of the same types
	   - it makes logic dependencies physiacal. It means if in Order we have customerId and in Customer we have the same information that if we change it in one place and won't change in the second than with Type identifier compiler will throw errors. The dependency is also physicall not only logical!!
	   - with type identifgier we can validate our Id
	   - Conclusion is that it;s good to use type identifier identity in our Entity
9. When and where to determine the ID of an entity [ARTICLE](https://matthiasnoback.nl/2018/05/when-and-where-to-determine-the-id-of-an-entity/)
	   -interesting design is to pass to the value object the id we want to declare. Because enity should not be aware of what id should be next in order. so we can implement in repository layer method nextIdentity() and from there take the next value which can be designed different for each storage system
10. Domain Driven Design - Q&A Sławomir Sobótka [YOUTUBE](https://www.youtube.com/watch?v=do-xqIbKZ_8)
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
11. What are factories in DDD [ARTICLE](https://culttt.com/2014/12/24/factories-domain-driven-design)
	- Why to use factories at all
		- standardises the instantiation of an object
		- provide encapsulation
		- object should not be responsible for its own creation
		- it important layer of abstraction
		- 



13. Problemy wynikające z modularyzacji - Łukasz Szydło
	--TODOoOOOOOOOOO
1. Architektoniczne antywzorce czyli na co chorują aplikacje - Łukasz Szydło


4. https://www.bottega.com.pl/pdf/materialy/ddd/ddd1.pdf
5. https://matthiasnoback.nl/2017/08/layers-ports-and-adapters-part-2-layers/
6. https://odrotbohm.de/2020/03/Implementing-DDD-Building-Blocks-in-Java/