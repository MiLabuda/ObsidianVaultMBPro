
1. Get your hands dirty on clean architecture
2. Valerina Jemuić
3. Hexagonal Architecture by example [ARTICLE](https://blog.allegro.tech/2020/05/hexagonal-architecture-by-example.html)
	- due to hexagonal architecture no adapter method needs to be public, so the i.e restcontroller doesn't need to be public. Because NO OTHER bean can depend on it
	- Each adapter works on it own data model, so you always want to translate from, or of. and not "to". So always remember to use rather 
	  ![[Pasted image 20240525163149.png]]
  4. Looking for an answer for "How can i test anemic model in hexagon architecture" i found an antipattern which is described here:
    Anemic Domain Model Antipattern - [ARTICLE](https://martinfowler.com/bliki/AnemicDomainModel.html) - FANTASTIC article that opened my eye that the other ways of planning my domain objects exists. I will put my mind into creating FAT domain models with proper behaviour and make the Service layer thinner. It's great to know that the anemic model is ANTIPATTERN 
5. WJUG #211 # Modularity and hexagonal architecture in real life: Jakub Nabrdalik [YOUTUBE](https://www.youtube.com/watch?v=ILBX9fa9aJo)
	- The protocol for dying[ARTICLE]
	- The video says a lot about package scope on the example of hexagon. CONTROLER DOESN'T NEED TO BE PUBLIC. we don't use it anywhere
	- CQRS = Command (POST,PUT) + Querry (GET) np mozna uzywac Article i ArticleQueryDTO jedno do zapisywania, inne tylko dowyciągania
	- Its very important to do Dynamic Analyze, it means to draw our microservices or modules and how they comunicate with each other
	- Summary, use package-scope. Every public method treat as it is published for user

While digesting more and more hexagon architecture I know i need to know more about DDD, because I'm missing some key names and definitions from DDD to be more fluent with articles on hexagon. I think that people often choose hexagon as practical implementation of DDD.

6. Domain-driven design needn't be hard. Here's how to start[ARTICLE](https://www.thoughtworks.com/insights/blog/domain-driven-design-neednt-be-hard-heres-how-start)
	- 