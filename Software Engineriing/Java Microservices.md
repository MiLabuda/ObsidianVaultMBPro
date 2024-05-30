
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

