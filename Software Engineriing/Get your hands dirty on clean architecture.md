 



8. Mapping between boundaries
	- No mapping strategy:
		- ![[Pasted image 20240613195845.png]]
		- no mapping strategy vs mapping strategy
		- no mapping strategy is when we use domain model i.e "Account" in web adapters in domain services and persistence adapters out. That violates single responsibility principle. But in some cases it can be done this way. But this can quickly be violated with some annotation from the web layer and maybe some annotation of orms framework etc.
		- No mapping strategy is perfectly valid when all layers need just the same model
		- mapping strategy can be changed while the project is evolving
	- Two-way mapping strategy
		- ![[Pasted image 20240613200030.png]]
		- the two-way mapping strategy - a strategy where each layer has its own model. That more aligned with DDD i think
		- with two-way mapping strategy is named that because eacch layer maps the models in both ways in and out.
		- in two-ways the web layer has model optimised best for presenting the data, the domain layer has model optimisied for best utilitizing use case and persistance layer has model optimised for best performing data base actions
		- in two ways mapping is responsibility of in and out adapters. which map for the domain, and the domain is not notified about anything, doesnt know anything. 
		- drawbacks for his is a lot of boilerplate and headeache with mapping or using mapping library.
		- another drawback is that our domain are crossing the layer boundaries. and its being used in in and out adapters for mapping purposes.
	- Full mapping strategy
		- ![[Pasted image 20240613200121.png]]
		- this mapping strategy introduce separate input and output for every model per operation. i.e every usecase has its command
		- such commands make interfaces of every usecase very explicit, well described
		- It doesn't have to be used everywhere. In example its best suited for mapping between web and application layer, but worse between application and persistance because it looks then as overkill
		- authors says that he recommends using it as an input the commands. And for the output just use the domain Model. (not wuite understood that - but i suppose it means something like this
		  public Mail sendMailUseCase(SendMailCommand){} gdzie w inpucie mamy command a w outpucie juz zwykly mail domenowy a nie command)
	- One way mapping strategy
		- ![[Pasted image 20240613200751.png]]
		- in this strategy all models across the layers implement the same interface like i.e AccountState which encapsulates the state of the domain model by providing getter methods for the relevant attributes.
		- the outer layers can decide if they can implement the state or map it to their own model
		- and domain implementation of the state can be rich model and have its logic that can be used by services in domain
		- This can be conceptually hard mapping strategy 
		- it plays the best when the models across the layers are similar and i.e web layer doesnt have to map to its own model because they can use state
9. Assembling the application
	- We need the configuration component that is neutral to our architecture and has dependency to all classes in order to instantiate them
	- ![[Pasted image 20240615101102.png]]
	- Assembling the application fro mthe config component with basic assembling is most basic way, we do not use dependency injection framework for this
	-  this is going to be much much more complicated with bigger systems because it generates a lot of code. 
	- Also everything is public so it would be possible from UseCase to call persistance methods with avoiding core services. it's not so good for us
	- Another way is to assemble it with Spring classpath scanning. (Scans all classes for annotations like @Component, @Bean, @Service @RestController and so on)
	- With spring the result of assembling all of the application with its dependencies is application context. The application context contains all beans that finally create application
	- We can also create our own annotations for Spring ike @PersistenceAdapter and so on
	- It has cons like it's invasive. We must put the annotation inside our classes
	- In normal developments it's not such a big problem, but i.e in libraries or frameworks its a no go because we don't want to encumber our users with spring
	- another con is that sometimes spring magic can happen. The bad magic like we miss some class which happens to be in configuration and it changes the state of the app and it's then very hard to debug
	- Another solution is to assemble the application via Spring Java config which gives you more control over your application
	- @EnableJpaRepositories placed on the main class makes you starting always the JPA, and when you put it onto the configClass then you have more control over when you want to start with JPA, because sometimes for test you might not need the persistance and then we can start without it
10. Enforcing architecture boundaries
	- The author highlight the importance that the dependencies always should point inwards (inside the circle) never outwards this is forbidden in ddd and should not be used
	- ArchUnit is a great tool for Java for ensuring that the boundaries of our architecture are strong and are not broken
	- One of the main features of the build tools like Maven of gradle are dependency resolution, a build tool first checks whether all artifacts that the code base depends on are available, if not then try to load them from artifact repository and if it fails the build will fail as well
	- Author says a lot about splitting the application into separate jars as a way to ensure the proper boundaries between layers in our architecture
11. Taking shortcuts conciously
	- broken-windows-theory that applies also to writing code
	- The responsibility of starting clean is big, because when we keep the code clean the chances are bigger that the code quality will mantain on high level
	- Sharing models between use cases --> if two or more usecases share the models then they share the reason to change "S" in solid. We should try our best to keep the commands and querries model separated between usecases both input and output (requests and responses)
	- The situation when we want to have the same models are when requirement is the same. I.e when we must to change something then the change will occur in both of the usecases and therefore models --> This is shortcut
	- Using domain entities as input or output models - this we should avoid but its another shortcut that we can take when implementing evry simple create or update cases 
	- Skipping incoming ports, another shortcut. We should avoid this Without the in ports we can just directly call the applicationService, but this way we have reduced a layer of abstraction between incoming adapters and the application layer. Very big advantage of the incoming ports is that we can look and instaltly know what the method we can use and what is the contract there, instead of searching of the internals of application service class
	- Skipping application services. While it is tempting to do so in CRUD systems. we should avoid such operations because therefore we need to have shared model between the web and persistence adapters which will be our domain entity. and sharing domain entity should be a NO GO. Also we no longer have usecase in our application core and when the complexity of the system gtows it is often reason to add logic into adapters instead our core layer
	- Evolving domain code free from external influence is the single most important argument for the hexagonal architecture style.