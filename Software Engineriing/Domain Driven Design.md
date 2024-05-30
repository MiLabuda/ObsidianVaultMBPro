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



1. Domain Driven Design - Q&A Sławomir Sobótka YOUTUBE
		--TODOoOOOOOOOOO
1. Problemy wynikające z modularyzacji - Łukasz Szydło
	--TODOoOOOOOOOOO
1. Architektoniczne antywzorce czyli na co chorują aplikacje - Łukasz Szydło