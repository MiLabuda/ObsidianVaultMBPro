
Java monolith 

monolith based on experience can turn with many different programmers/teams work for a long time under high pressure and unclear requirements and then seethat it's something than anyone is afraid of deploying to the server

Why we want to use micro-services?
because of the characteristic of monolith it is sometimes a smart way to extract some modules of the code into its own maven/gradle so it becomes separate Java process

When it comes to microservices we can choose one of two options if it comes to communication between micro-services.
- synchronous calls (HTTP)/ REST
- asynchronous messaging

We can try to create new micro-services based on already existing monolith or we can start building microservices from scratch (greenfield)
We should not model our microservices on domain boundaries, like if we send the message from the doctor to the insurance company and then send the response we shouldn't make the 6 microservices with its own database 

Good tolls/libraries for 
- synchronous: Java http client, Apache http client, OkHttp, FeignClient
- asynchronous: ActiveMQ, RabbitMQ, Kafka
- testing:  [Testcontainers](https://www.testcontainers.org/),  [Junit](https://junit.org/junit5/), [TestNG](https://testng.org/doc/) to [AssertJ](https://joel-costigliola.github.io/assertj/) and [Mockito](https://site.mockito.org/).


1 Richardson, C. (2019). Microservice Patterns: With Examples in Java. New York: Manning Publications.
	1. Escaping monolithic hell
		- Scale cube 
		  ![[Pasted image 20240711211018.png]]
		-  X-axis
		  scaling load balances requests across multiple instances (multiplying instances. i.e our prod has 3 instances)
		- Z-axis 
		    scaling routes requests based on an attribute of the requests (i.e by userId)
		- Y-axis scaling functionality decomposes an application into services
		- Microservices using dumb pipes such as message brokers or direct service to service communication, using lighweight protocols such as REST or gRPC, has its own models and database per service.
		- Microservices enable the continuos delivery and deployment of large complex applications.
			- it has testability required by continuos delivery/deployment - automated tests are much easier to write and execute.
			- it has deployability required by continuos delivery/deployment - each service can be deployed separately from others. The team can deploy the microservice without interupting other teams work
			- it enables development teams to be autonomous and loosely coupled
		- Microservices consists of services small and easily maintained
			- Image
			  ![[Pasted image 20240711224323.png]]
		- They are independently scalable
		  eacch microservice can be scaled independently or other services but also it can be deployed on hardware that best suits the needs of given microservice
		- Easily adopt new technologies, there is so much easier in small microservice to try new language or framwork instead migrating of whole monolith
		- Drawbacks of microservices:
			- finding the right set of services is hard
			  if implemented incorectly we can get distributed monolith which has the drawbacks of both microservices and monolith
			- distributed systems are complex, which makes development testing and deployment more difficult
			- deploying features that has scope for multiple services requires coordination
			- deciding when to adopt the microservice is difficult
		- Author gives much credit for patterns and pattern language
			- Pattern is a reusable solution to a problem that occurs in a particular context. 
			- pattern language is a collection of related patterns that solve problems within particular domain
			- Pattern consist of three sections:
				- Forces:
					- issues that you must address when solving a problem in a given context. It also can have a lot of issues and solving them all won't be possible. That's why it's good to have them written down to see which one are most important for us in this specific context
				- Resulting context:
					- describes the consequences of applying the pattern. It consists of three parts:
						- benefits
						- drawbacks
						- issues
				- Related patterns:
					- this is the relationship between the pattern and other pattens. There are five types of relationships:
						- predecessor - a pattern that motivates the need for this pattern. the microservice architecture pattern is the predecessor to the rest of the patterns in the pattern language, except monolithic architecture pattern.
						- successor - a pattern that solves an issue that has been introduced by this pattern. if you apply the microservice pattern you must then apply many successor patterns including service discovery patterns and the circuit breaker pattern.
						- alternative - a pattern that provides an alternative solution to this pattern. For example the monolithic architecture pattern and the microservice architecture pattern are alternative ways of architecting an application. You pick just one.
						- generalization - A pattern that is a general solution to a problem. For example different implementation Single Service Per Host pattern
						- specialization - specialized form of particular pattern. For example Deploy a service as a container pattern is a specialization of single service per host.
				- Patterns that are introduced by microservices
					- infrastructure - solve infrastructure problems that are out of development scope
					- application infrastructure - these are for infrastructure issues that impact development
					- application patterns - these solve problems faced by developers
			- Patterns for decomposing application into microservices
				- decompose by business capability
				- decompose by subdomain (DDD)
			- Communication patterns
				- communication style - what kind of  ipc mechanism should you use
				- discovery - how does a client of a service determine the ip address of a service instance, so that for example it makes and http request
				- reliability - how can you ensure that communication between services is reliable even though services can be unavailable
				- transactional messaging - how should you integrate the sending of messages and publishing of events with database transactions that update business data?
				- external API - how do clients of your application communicate with the services?
				- ![[Pasted image 20240715185345.png]]
			- Patterns for querying data
				- ![[Pasted image 20240715185820.png]]
			- Service deployment patterns
			- Observability patterns
				-  health check api - expose and endpoint that returns the heath of the service
				- log aggregation - log service activity and write logs into a centralized logging server, which provides searching and allerting
				- distributed tracing - assign eac hexternal request a unique ID and trace requests as the flow between services
				- exception tracking - report exceptions to an exception tracking service which deduplicates exceptions, allerts developers and tracks the resolution of each exception
				- application metrics - maintain metrics such as counters and gauges and expose them to a metric server
				- audit logging - log users actions
			- Patterns for the automated testing of services
				- consumer-driven contract test - verify that a service meets the expecgtations of its clients
				- consumer-side contract test - verify that the client of a service can communicate with the service
				- service component test - test a service in isolation 
			- Patterns for handling cross-cutting concerns
				- in microservice architecture there are numerous concerns that every service must implement, including the observability patterns and discovery patterns. It must also implement externalized configuration pattern which supplies configuration parameters such as database credentials to as service at runtime. When developing service from the beginning it would be too time consuming to implement it from scratch instead its better to use chasis pattern and build services on top of a framework that handles these concerns
			- Security patterns
				- authenticate user in api gateway and pass i.e token for services
			- "Continuos delivery" 
				- is the ability to get changes of all types including new features, configuration changes, bug fixes and experiments into production, or into the hands of users safely and quickly in a sustainable way
				- software is always releasable
				- includes automated testing
				- continuos deployment takes continuos delivery one step further in the pracise of automatically deploying an releasable code into production. High performing organiuzations that practise continuos deployment deploy multiple times per day into production. have fever production outgages, and recover quickly when they occur
				- move fast without breaking things!
			- Software architecture
				- its a high level structure which consists of constituent parts and the dependencies between those parts. As you'll see in this section an application architecture is multidimensional so there are multiple ways to describe it. It is important because it determines the application quality, scalability, security, reliability. At the top is fast and safe delivery of software
				- The 4+1 model of software architecture [ARTICLE](https://www.cs.ubc.ca/~gregor/teaching/papers/4+1view-architecture.pdf)
					- Logical view the software elements that are created by develiopers. In object oriented languages these elements are classes and packages(inheritance, associations, depends-on)
					- Implementation view - the output of the build system, This view consists of modules, which represent packaged code, and components. In java a module is a JAR file and a component is typically a WAR file, 
					- Process view - the components at runtime. Each element is a process, and the relations between processes represent interprocess communication
					- Deployment - how the processes are mapped to machines, The elements in this view consist of machines and processes. The relations between machines represent networking.
					- ![[Pasted image 20240715194514.png]]
					- Why it matters
						- because its responsible for the quality of service -ities (scalabilities, securities, reliabilities and so on)
						- 