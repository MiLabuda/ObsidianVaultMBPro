
1. Microservices with Spring
	- Conway's law - Organization which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations
	- Spring boot key features
		- an embedded web server to avoid complexity in deploying the application (Tomcat - default), Jetty, Undertow. This is very important part of spring boot, chosen web server is part of the deployable JAR, for spring boot application the only requirement to deploy the app is to have java installed on the server
		- suggested configuration to start quickly with development
		- automatic configuration for spring functionality, whenever possible
		- wide range of features ready for production, such as metrics, security, status veryfication, externalized configuration and so on
	- Spring boot features for microservices:
		- reduces development time and increases efficiency and productivity
		- offers an embedded htttp server to run web applications
		- allows you to avoid writing a lot of boilerplate
		- faciliates integration with the spring ecosystem(includes spring data, spring security, spring cloud)
		- provides various of development plugins
	- Spring boot with the common REST endpoint is doing 
		- route mapping based on http request, http verb, the url and potential parameters. route mapping this to spring rest controller class
		- next it will map any parameters and pass to java method
		- once all the data has ben mapped spring boot will execute the business logic inside the endpoint
		- once the business logic is executed it will convert java object to json
	- Cloud computing models:
		- Infrastructure as a service (IaaS) - the vendor provides the infrastructure that lets you access computing resources, but developer is responsible for everything related to the maintenance of the infrastructure
		- Container as a service (CaaS) - you deploy your lighweight portable virtual container (i.e Docker) to the cloud providers, then cloud provider runs the virtual server the containers are running on, and provide tools for building eploying monitoring and scaling containers
		- Platform as a Service (PaaS) - This model provides a platform and an environment and developer can focus only on developing of the app.
		- Function as a Service (FaaS) - serverless architecture, despite the name, the architecture doesnt mean that running specific code is done without server. It means the way of executing functionalities in the cloud in which the vendor provides all the required servers
		- Software as a Service (SaaS) - this model allows user to use specific application without having to deploy or to maintaint it. in most cases its accessed through web browser. Everything is managed through service provider
		- ![[Cloud_computing_models.png]]
	- Microservice can be deployed into
		- physical server - only few organization do this because this physical servers are constrained. you cant quickly add the server capacity, so the scaling is hard and expensive
		- virtual machine images - VMs are the hearth and soul of every cloud providers
		- virtual containers - rather than deploying your microservice to a full VM< many developers deploy services as docker containers to the cloud. Virtual containers run isede a vm and using a virtual container you can segregate a singe vm into a series of self contained processes that share the same image. multiple instances of the same service can be quickly set up
	- Scaling an application
		- horizontally - adding more machines or instances to your application. Adding more servers to distribute the load
		- vertically - increasing capacity of a single machine or server, this could mean adding cpus, memory, or storage to the existing server
	- Good written microservice should be:
		- right-sized - how do you make sure the service is focused on one are of responsibility
		- location-transparent - how do you manage the physical location so service instances can be added and removed without impacting service clients
		- resilient - how do you make sure when there is a problem with a service, service clients fail fast?
		- repetable - how do you ensure that every time a new service instance is started it always has the same code and configuration as existing instances?
		- scalable - how do you ensure that your normal application can scale quickly with minimal dependencies between services
	- Core microservice development pattern
		- service granularity - what is the right level of responsibility the service should have
		- communication protocols - how you client and service communicate data back and forth
		- interface design - how you are going to expose your service endpoint to clients
		- configuration management - how your services manage their application specific configuration so that the code an dconfiguration are independent entities
		- event processing - how you can use events to commynicate state and data changes between services
	- Microservice routing patterns 
		- service discovery - by this pattern you can make your microservice discoverable so client applications can find them without having the location ofthe service hardcoded into their application its uses service registry for keeping track of
		  example of this is 
			- Netflix Eureka
			- Apache zookeeper
			- consul
		- Service routing - with an API gateway we can provide single point of entry for every of our services so that security policies and routing rules are applied unoformly to multiple services and service instance. It is done with Spring Cloud API Gateway
		- ![[Pasted image 20240816160114.png]]
	- Microservice client resilliency patterns
		- Client-side load balancing pattern - How we cache the location of our service instances on the service so that calls to multiple instances of a microservice are load balanced to all the health instances of that microservice
		- Circuit breaker pattern - How we prevent a client from continuing to call a service that's failing, when a service is running slowly it consumes resources on client side. We want these microservices to fail fast so the calling client can quiclly respond and take appropriate actions
		- Fallback pattern - when a service fail, how do we provide mechanism that allows the service client to continue its work through alternative way
		- Bulkhead pattern - microservice applications use multiple distributed resources to carry out their work (Dont understand it much right now)
		- ![[Pasted image 20240816161504.png]]
	- Microservice security patterns
		- Authentication - how we determine the service client calling the service is who they say they are
		- Authorization - How do you determine whether the service client calling a microservice is allowed to undertake the action they're trying to take
		- Credential management and propagarion - How we prevent a service client from constantly having to present their credentials for service calls involved in a transaction
		- ![[Pasted image 20240816161714.png]]
		- OAuth 2.0
	- Microservice logging and tracing patterns
		- Microservice architecture is much more difficult to debug and trace and monitor the issues. 
		- Log correlation pattern - How we tie together all the logs produced between services for a single user transaction. This pattern can allow for implementing correlation ID, which is unique identifier that carried accross all servie calls in a transaction and that can be used to tie all the logs related together up
		- Log aggregation - we will check how to pull all of the logs produced by microservice and its instances into single queryable database across all the services involved and understand the pergformance characteristics of the services into transaction
		- Microservice tracing - how we visualize the flow of a client transaction across all the services involved 
		- ![[Pasted image 20240816162515.png]]
	- Application metrics pattern
		- this pattern is responsible for how the application is going to monitor metrics and warn of possible causes of failure of the application. this patterns show how the metrics service is responsible for scraping, storing, querrying data in order to prevent potential performance issues in our services
			- Metrics - how we create critical information about the health of the application
			- Metrics service - where we can store and query the application metrics
			- MEtrics visualization suite - Where we can visualize business related time data for the application and infrastructure
		- Styles of obtaining metrics
			- Push - the service instance invokes a service API exposed by the metrics service in order to send the application data
			- Pull - the metrics service asks or queries a function to fetch the application data
	- Microservice build-deployment patterns
		- Build and deployment pipelines - how we crete a repeatable build and deployment process that emphasizes one button build and deployment to any enronment in you organizaiton
		- Infrastructure as a code - how we treat ptovisioning of our services as code that can be executed and managed under source control
		- Immutable servers - once a micrtoservice image is created how we ensure that it's never changed after it has been deployed
		- Phoenix servers - how we ensure that servers that run individuall containers get torn down on a regular basis and re-created from an immutable image. The longe server is running the more opportunity there is for configuration drifft
		- ![[Pasted image 20240816163630.png]]
2. Exploring the microservices world with Spring Cloud 
	- The more distributed a system is, the more places it can fail.
	- Spring cloud overview
	  ![[Pasted image 20240816214602.png]]
	- Spring Cloud Config 
		- It handles the management of the application configuration data through a centralized service. Our application configuration data (especially environment specific configuration data) is clearly separated from your deployeed microservice. Spring cloud config has its own property management repository but also integrates with open source projects like
			- Git - version control system, Spring Cloud COnfig integrates with a Git backend repository and reads the application configuration from the repository
			- Consul - open source service discovery that allows service instances to register thmselves with a service, service clients can then quyery Consul to find the location of their service instances.
			- Eureka - open source netflix project, like Consul offers similar service discoery capabilities. Eureka also has a key-value database that can be used with Spring Cloud COnfig
	- Spring Cloud Service Discovery
		- with this you can abstract away the physical location (IP or server name) of where the servers are deployed from the clients consuing the serviceSpring Cloud Service discovery also handles the registration and deregistration of service instances as these are started and shyt down. Spring Cloud Service can be implemented using
			- Consul
			- Zookeper
			- Eureka ( the most popular one)
	- Spring Cloud LoadBalancer and Resilience4J
	- Spring Cloud API Gateway
	- Spring Cloud Stream
	- Spring Cloud Sleuth
	- Spring Cloud Security
	- Cloud computing
		- cloud-ready application - app that was once used on a computer or on an onsite server. With the arrival of the cloud, these types of application have moved from static to dynamic environments with the aim of running in the cloud
		- cloud-native application - designed specifically for a cloud computing architecture to take advatage of all of its benefits and services
			- Four principles of native cloud development are
				- DevOps - it refers to a methodology that focuses on communication, collaboration and integration among software developers
				- Microservices are small, loosely coupled, distributed services - these allow you to take a large application and decompose it into easy-to-manage components with narrowly defined responsibilities
				- Continuos delivery - its a process of delivering software in automated ways, allowing short-term deliveries to a production environment
				- Containers are a natural extension of deploying your microservices on a virtual machine (VM) image - instead of deploying service to full vm, many developers deploy th services as docker containers to the cloud
			- Microservices by definition is cloud-native
		- Heroku's 12 factor manifesto - 12 best practises [https://12factor.net/]
		  ![[Pasted image 20240816222034.png]]
		1. Codebase 
			- with this practise, each microservice should have a single, source controlled codebase. Server provisioning information should be in codebase as well. The codebase can have multiple instances of deployment environments (development, staging production) but its not shared with any other microseervice
		2. Dependencies 
			- ![[Pasted image 20240816225900.png]]
		3. Config 
			- never add embedded configurations to your source coede! Instead its best to maintain your configuration completely separate from your deployable microservice
			- This is how environment configuration should look
			  ![[Pasted image 20240816230043.png]]
		4. Backing services 
			- We should be able to swap our deployment implementations between local and third-party connections without any changes to the application code
			- ![[Pasted image 20240816230247.png]]
		5. Build, release, run
			-  we should always remember to kkep our byild, release, and run stages of an application deployment completely separated. We should be able to buil microservices that are independent of the environment on which they run. 
			- Once the code is built any runtime changes need to go back to the build process and be redeployed. A built process is immutable and cannot be changed
			- The realese phase is in charge og combining the built service with a specific configuration for each targeted environment
			  ![[Pasted image 20240816230952.png]]
		6. Processes
			- Our microservices should always be stateless and should only contain the necessary information to carry out the requested transaction. Microservices can be killed and replaced at any time without the feat that a loss of a service instance will result in data loss. If there is a specific requirement to store  a state it must be done with in memory cache such as redis or a backing database
			- ![[Pasted image 20240816231231.png]]
		7. Port binding
			- it means to publish services through a specific port. In a microservices architecture a microservice is completely self-contained with the run-time engine for the service packaged in the service executable. 
			- We should be able to run the service without the need for a separate web or application server. The server should start by itself on the command line and be accessed immediately through an exposed HTTP port
		8. Concurrency
			- cloud-native application should scale out using the process model. That means rather than making single process larger, we can create multiple processes and then distribute the service's load among different processes
			- vertical scaling (scale up) - refers to increasing the hardware infrastructure.
			- horizontal scaling (scaling out) - refers to adding more instances of the application.
			- It's better to scale out and launch more microservice instances rather than scaling up
			- ![[Pasted image 20240817105912.png]]
		9. Disposability
			- microservices need to start and stop on demand in order for elastic scaling and to quickly deploy code or configuration changes
		10. Dev/prod parity
			- having different environments (development, staging, production) as analogous as possible. The environments should always contain similar versions of deployed code
		11. Logs
			- logs are stream of events. logs should be managed by tools as Logstash which collects the logs and write those to cental location
			- The microservice should never be concerned about the mechanisms of how this happens
			- ![[Pasted image 20240817110652.png]]
		12. Admin processes
			- developers when doing administrative tasks for their services(data migration), these task should never be done ad hoc on the server it should be done via scripts that are managed and maintained through a source code repository. 
			- The scripts should be repeatable and non-changing
3. Building microservices with Spring Boot
	- Architects story:
		- decomposing the business problem
			- rules for decomposing the businees problem
			- describe the business problem and notice the nouns you use to describe it
			- pay attention to the verbs
			- look for data cohesion
		- establishing service graularity
			- rules for establishing granularity
			- it's better to start with broader microservice and refactor it to smaller ones
			- focus first on how our services interact with one another
			- service responsibilities change over time as our understanding of the problem domain grows
			- SMELLS OF TOO BIG SIZE AT MICROSERVICES
			- service with too many responsibilities
			- service that manages data across a large number of tables (NO MORE THAN 3-5 DB TABLES)
			- service with too many test cases (hundreds of unit tests is too much)
			- SMELLS OF TOO LITTLE SIZE AT MICROSERVICES
			- microservices in one part of business logic breeds like rabbits
			- microservices are heavily interpendent on one another
			- microservices become a collection of simple CRUD services
		- defining the service interfaces
			- rules for defining the service interfaces
			- embrace the REST philosophy (Appendix A)
			- use URIs to communicate intent
			- use JSON for your requests and responses
			- use HTTP status codes to communicate results
	- When not to use microservices
		- servers or containers are growing upo in numbers and your organization must take the responsibility and have the resources to properly work with these numbers on cloud
		- if your application will be small, scalability not all that important, resilient as well then you should not build microservices
	- Developers story:
		- @RestController annotation in Spring Boot authomatically serialize/deserialize service requests/responses via JSON
		- Guideline for naming service endpoints:
			- use a clear URL names that establish what resource the service represents
			- use the url to establish relationships between resources
			- establish a versioning scheme for urls early. The URL and its corresponding endpoints represent a contract between the service owner and the consumer of the service IMPORTANT, use versioning!!!
		- Adding internationalization to rest controller
			- adding LocaleResolver and ResourceBundleMessageSource beans
			  ![[Pasted image 20240819190523.png]]
			- @RequstHeader annotation for retrieving on the endpoint the Locale value
			- Implement HATEOAS to display related links - with spring HATEOAS you can qukcliy create model classes for links to resource representation models. It also provides a link builder API to create specific links that point to Spring MVC Controller methods.
				- To add HATEOAS you need add dependency
				- update the model to extend from RepresentationModel<'T'>
				- ![[Pasted image 20240819224747.png]]
	- Devops story:
		- Microservice should be self-contained - it should be independently deployable with multiple instances of the service being started up and torn down
		- microservice should be configurable - when a service starts up it should read the data it needs to configure itself from a central location or have its configuration information passed on as environments variables
		- microservice instance needs to be transparent to the client - client shouldm never know the exact location of a service, microservice client should talk to a service discovery agent that allows the application to locarte an instance of a microservice without having to know its physical location
		- microservice should communicate its health
		- 4 principles of devops:
			- service assembly - how you package and deploy your service to guarantee repeatability and consistency tso that the same service code and run time are deployed exactly the same way
			  ![[Pasted image 20240819225949.png]]
			- service bootsraping - how you separate you application and environment specific configuration from the run-time code so that you can start and deploy a microservice instance quickly in any environment  without human intervention
			  ![[Pasted image 20240819230130.png]]
			- service registration/discovery - when a new microservice instance is deployed how tou make new service instance discoverable by other application clients
			  ![[Pasted image 20240819230546.png]]
			- service monitoring - in a microservice environment its common for multiple instances of the same service to be running due to high avaiability needs. from a devops perspective you need to monitor microservice instances and ensure that any faults are routed around failing service instances
			  ![[Pasted image 20240819230642.png]]
		- Spring boot actuator - it's great way to expose healtchecks in spring boot application, we can get the basic data, about what's up
		  ![[Pasted image 20240819231020.png]]
		  and we can configure it with application properties
		  `management.endpoints.web.base-path=/
			`management.endpoints.enabled-by-default=false
			`management.endpoint.health.enabled=true
			`management.endpoint.health.show-details=always
			`management.health.db.enabled=false
			`management.health.diskspace.enabled=true
4. Welcome to Docker
	- 
5. Controlling your configuration with the Spring Cloud Configuration Server
	- Best pracitses for deploying microservices
		- completly separate the configuration of an application from the actual code being deployed 
		- build immutable application images that never change as these are promoted through your environments
		- inject any application configuration information at server startup through either environment variables or centralised repository that the microservices read on startup 
	- Four principle to follow while deploying
		- segregate - we need completely separate the service configuration information from the actual physical deployment of a service, In fact application configuration shouldn't be deployed with the service instance. Instead configuration information should either be passed as environment variables to starting service or read from a centralised repository when the service starts.
		- abstract - we also need to aabstract access to configuration data behind a service interface. Instead of writing code that directly reads the service repository whether file-based or jdbc database we should use rest based json service to retriebe the application configuration data
		- centralize - because a cloud based application might literally have hundreds of services its critical to minimize the number of different repositories used to hold configuration data. centralize your application configuration into as few repositories as possible
		- harden - because your application ocnfiguration information is going to be comoopletelY segregated from you deployed service and centralized it's critical that the solution you utilize and implement be highly available and redundant
	- Setting up the configuration microservice
		- ![[Pasted image 20240820234304.png]]
		- ![[Pasted image 20240820234313.png]]
		- ![[Pasted image 20240820234324.png]]
		- Spring Cloud config Server with filesystem
		- Instruction on how to implement spring cloud config server with filesystem (125-135)
		- Spring Boot Actuator offers a @RefreshScope annotaiton that allows development team to access a refresh endpoint that will force the spring boot aplication to reread its application configuration
			- This annotation only reloads the custom Spring properties you have in your application. Items like database config used by Spring Data wony be reloaded
		- One thing to consider before dynamically change properties is that you can have multiple instances of the same running service. you will need then refresh all of those services with their new application configurations.
			- Spring Cloud Config offers a push-based mechanism called Spring Cloud Bus, that allows SCC to publish to all the client using the service where a change occurs. Spring Cloud Bus requires an extra piee of running middleware: RabbitMQ, this is an extremely useful means of detecting changes but not all spring cloud config backends support the push mecahnism 
		- Spring Cloud Config with Git
			- \
			- \
6. On service discovery
	- service discovery lets the application team to quickly scale horizontally the number of service imstamces running in an envitonment 
	- the other benefit or service discovery is that it helps increase applocation relisciency, by detecting the unhealthy or unavailable microservices it can tore it down and restore
	- Where is my service 
		- traditional service location resolution model uses a DNS and a load balancer 
		- ![[20240822154314.png]]
	
		-  this solution has some drawback then talking about administrating microservice applications - while the load balancer can be made highly available, its a single point of failure for your entire infrastructure, if the load balancer goes down, every application relying on it goes down too. 
		- centralizing your services into a single cluster of load balancers limits your ability to scale horizontally toue load balancing infrastructure across multiple servers 
		- most traditional load balancers are statically managed - they arent designed for dast registration and deregistration of services 
		- because a load balancer acts as a proxy to the services, service consumer requests need to have them mapped to the physical services 
	- cloud-based service discovery 
		- highly available - if a node in cluster becomes unavailable, other nodes in the cluster should be able to take over 
		- peer-to-peer - each node in the service discovery cluster shares the state of a service instance 
		- load balanced - service discovery client should cache service information locally, local caching allows for gradual degradation of the service discovery feature so that if the service discovery service becomes unavailable, applications can still function and locate the services based on the information maintained in their local cache 
		- fault tolerant - service discovery needs to detect when a service instance isn't healthy and remove that instance from the list of available services 
	- the architecture of service discovery 
		- service registration - how a service registers with the service discovery agent
		- client lookup of service address - how a service client looks up service information 
		- information sharing - how nodes share service information 
		- health monitoring - how services communicate their health back to the service discovery agent - ![[20240822160329.png]]
		
		- Logical name is shared across all instances, its like name in a phone book, service OrderService knows only that he needs the data from user service, and will contact service discovery and will get the exact IP address 
		- Two models of service discovery 
			- first approach client relies on service discovery engine to resolve service locations each time a service is called. (not so good, because service is strongly dependent on the service discovery microservice) 
			- other approach is client-side load balancing (i.e Eureka) if the service instance goes down it is removed from the registry, once that is done the client--side load balancer updates itself without manual intervention by establishing constant communication with the registry service - ![[20240822162145.png]]
			- in this approach client service doesn't need to contact service registry each time, it contacts service registry only when it won't find the IP address in cache
		- When implementing Eureka we can use REST API or Eureka Dashboard to see the contents of registry 
7. When bad things happen: Resiliecy patterns with Spring Cloud and Resilience4j

	Reasons why sometimes it's hard to detect slow services:
	- service degradation can start out as intermittent and then build momentum.
	- calls to remote services are usuaally synchronous and don't cut short a long-running call
	- application are often designed to deal with complete failures of remote resources, not partial degradations
	4 patterns of resiliency while implementing microservices
	![[Pasted_image_20240905160755.png]]
	- Client side load balancing 
	- Client side load balancing is looking up all of the service individual instances from service discovery agent (like Netflix Eureka) and then caching the physical location it maintains.
	- Client side load balancer sits between the service client and the service consumer and therefore can detect if a service instance is throwing errors or behaving poorly
	- Circuit breaker - aka "Bezpiecznik, korek(elektryczny)"
	- when remote servie is called the circuit breaker monitors the requests and if it takes too long it will kill the request. If many calls will fail the circuit will turn of the instance preventing future calls to the failing remote resource
	- Fallback processing
	- when a remote service call fails, service consumer instead of generating exception will execute alternative path and will continue ith fallback data, for example we can send notification that you should try the request later or even use different behaviour when initial service fails
	- Bulkheads - aka "przegroda (like on ships)"
	- thread pools acts as the bulkheads, each remote resource is segregated and ssigned to a thread pool. If one service is responding slowly, the thread pool for that type of service call can become saturated and stop processing requests.
	- The circuit breaker patterns allow the remote calls to have possibility to:
		- fail fast
		- fail gracefully(with fallback)
		- recover seamlessly - periodically check to see if the resource being requested is back online
	- Circuit breaker - stops making requests when invoked service is failing
		- retry - retries a service when temporarily fails
		- bulkhead - limit the number of outgoing concurrent service requests to avoid overload
		- rate limit - limits the number of ingoing calls that service receives
		- fallback - sets alternative path for failing requests
	- TO BE CONTINUED
8. Service routing with Spring Cloud Gateway
	- ensuring critical behaviours across all microservices in our system might be very important but's it can have such implications:
		- it's challenging to implement these capabilities in each service consistently
		- pushing the responsibilities to implement cross-cutting concerns like security and logging down to the individual development teams greatly increases the odds that someone will not implement them properly or will forgot to do them at all
		- it's possible to create a hard dependency across all our services
	- Our gateway can be the single Policy Enforcment Point (PEP)
	- With Spring Cloud Gateway we will
		- put all service calls behind a single URL and map those calls using service discovery to their actual service instances
		- inject correlation IDs into every service call flowing through the service gateway
		- Inject the correlation ID returned from the HTTP respponse and send it back to the client
	- What is service gateway
		- ![[Pasted image 20240907222749.png]]
		- ![[Pasted image 20240907222758.png]]
	- Cross cutting concerns that can be implemented in gateway
		- Static routing - a service gateway places all service calls behind a single URL and API route. This simplifies development as we only have to know about one service endpoint for all of our services
		- Dynamic routing - a service gateway can inspect incoming service requests and based on the data from the incoming requests perform inteligent routing for the service calles. For instance, customers participating in a beta program might have all calls to a service routed to a specific cluster of services that are running a different version of code (SOME USERS MIGHT BE GETTING DIFFERENT VERSION OF OUR SYSTEM IF WE WANT)
		- Authentication and authorization - because all service calls route through a service gateway the service gateway is a natural place to check whether the callers of a service have authenticated themselves
		- Metric collection and logging - a service gateway can be used to collect metrics and log information as a service call passes through it. You can also use the service gateway to confirm that critical pieces of information are in place for user requests, thereby ensuring that logging is uniform. This doesn't mean that you shouldn't collect metrics from wtihin your individual services.
	- Introducing Spring CLoud Gateway
		- **Proxy:** A proxy acts as an intermediary between a client and the internet, forwarding requests from the client to the server and returning the server's response to the client.
		- **Reverse Proxy:** A reverse proxy sits in front of servers, handling incoming requests from clients, routing them to the appropriate server, and then sending the server's response back to the client.
		- Predicate and filter factories
		  ![[Pasted image 20240907234950.png]]
		  - Implementing first filters
		    ![[Pasted image 20240908074550.png]]
		- Putting things like utils and classes for filters into separate, shared fo arll library is a grey area when it comes to good practises. The heuristic can be to use it only for infrastructure-like code. Do not put any business related code there.
9. Securing your microservices
	- Multiple layer of protecting microservices system
		- the application layer - ensures that the proper user controls are in place so that we can validate that a user is who they say they are and that they have permission to do what they're trying to do
		- infrastructure - keeps the service running, patched and up to date to minimize the risk of vulnerabilities
		- a network layer - implements network access controls so that a service is only accessible through well-defined ports and only to a small number of authorized servers
	- OWASP dependency check - worth thing to apply, it will discover and inform us about vulnerabilities in dependencies which we use in projects
	- OAuth2.0
	- Keycloack
10. Event-driven architecture with Spring Cloud Stream
	- When talking about microservices and the usage of event and messages we have example of caching data from other microservices for optimisation pursposes we need to have in mind
		- cached data needs to be consistent across all INSTANCES of the licensing service. 
		- we cannot cache the organization data within the memory of the container hosting the licensing service
		- when organization record changes via an update or delete, we want the licensing service to recognize that there has been a state change in the organization service
	- Synchronous request-response approach to cache data and comunicate changes
		-   ![[Pasted image 20240911200657.png]]
		- the drawbacks of such approach
			- the organization and licensing servies are tightly coupled. this coupling introduces brittleness between the services
			- if the licensing service endpoint for invalidating the cachce changes, the organization service has to change. this approach is inflexible.
			- we can't add new consumers of the organization data without modifying the code on the organization service to verigy that it calls the licensing service to let it know about any changes.
	- Messaging way of communicating state between services
		- ![[Pasted image 20240911201753.png]]
		- Pros of using this approach
			- loose coupling (we don't have rest connections and can introduce changes (exception for serializing and deserializing jsons))
			- durability (we have messages in the broker even if one of the service will go down we still have some data left and our service won't break because of failed other service)
			- scalability (we can always add more consumers to consume data in our sysyem)
			- flexibility(the sender of message has no idea who is going to consume it)
		- Drawbacks of messaging architecture
			- message-handling semantics(require well understanding of things way more complicated then implementic producers and consumers but also require knowing the strategy in all circumstances)
			- message visibility
			- message choreography(code is no longer processed in linear fashion)
	- Spring Cloud Stream 
		- Spring Cloud Stream as an abstraction for messaging system in Spring, it provides the facade, and under the hood we can use one of the implementations like Kafka or RabbitMQ
		  ![[Pasted image 20240911204814.png]]
		  - Components of Spring Cloud to publish and consume messages
			  - Source
			  - Channel
			  - Binder
			  - Sink
11. Distributed tracing with Spring Cloud Sleuth and Zipkin
	- Spring CLoud Sleuth
		- transparently create and inject correlation id into our service calls if one doesnt exists
		- manage the propagation of correlation ids to outbound service calls so that the correlation id for a transaction is automatically added
		- add the correlation information to springs mdc loggin so that the generated correlation id is automatically logged by springs boot default sl4j and logback implementation
		- optionally publish the tracing informatiion in the service call to the zipkin distributed tracing platform
	- Implementing ELK stack (Elasticsearch, Logstash, Kibana)
		- ![[Pasted image 20240925225354.png]]
		- creating the logstash tcp adapter 
			we need to create adapter because logback by default stores the logs in plain text but to be able to send them to logstash they need to be transformed to json, we can do it 3ways
			- using the net.logstash.logback.encoder.LogstashEncoder class
			- using net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder class
			- Parsing the plain text log data with Logstash
			The 1st is the easiest but the second allows us for more configuration