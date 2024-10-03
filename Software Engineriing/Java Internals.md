1. What is Java
	- a computer programming language, that is 
		- object-oriented 
		- concurrent, 
		- platform independent
		- has garbage collector
	- Write Once, Run anywhere (WORA)
	- 1996 - James Gosling invents Java. Java is a relatively verbose, garbage collected, class based, statically typed, single dispatch, object oriented language with single implementation inheritance and multiple interface inheritance. Sun loudly heralds Java's novelty.
	- Main components of Java
		- JVM (Java Virtual Machine)
		- Compiler
		- Standard Library
2. Differences between JVM, JRE, JDK
	- JVM - execution environments which is responsible for Java core features like garbage collection, memory management and platform independence
	- JRE - Java Runtime Environment, it goal is to simply run the java application on the operating system
	- JDK - it contains JRE for running application but besides it also contains developer tools for creating apps, like compilers and debuggerr
3. Difference between int and Integer
	- int:
		- it's primitive data type
		- not allowed for generics
		- represents the plain number
		- it's size is 32 bits(4bytes)
		- default values 0;
		- faster performance because its simpler than object
	- Integer:
		- wrapper class for primitive int
		- additional functionality
		- require more memory than int
		- default value null
		- allowed for generics
		- instantation through constructor or autoboxing
4. What are wrapper classes
	- they let you work with primitives as reference types. Very usefull for generics, which allow only objects
	- we can i.e wrap primitives using Autoboxing into wrapper
	- Popular structure that uses generics is ArrayList<();, we cant put the primitives inside the collections, or sets
	- They allow for nullability, when we donot some value then we can return null instead of 0
5. What it means that Java is statically typed
	- It means that it requires explicit typ declaration before its used. This allow for erly identification on types related errors suchas types mismatches
	- it is type safe
	- predictability 
6. What is the concept of Object Oriented Programming
	- abstraction
	- encapsulation
	- polymorphism
	- inheritance
	- predefined types must be objects
	- user defined types must be objects
7.  Is Java 100% Object oriented? - NO
	- Java does not satisy all the OOP conditions (all predefined types must be objects) because it uses eight primitive data types (boolean, byte, char, int, float, double, long, short)
	- it has static keyword that lets the class own a method rather than actual object
8.  What is abstraction
	- abstraction is the process of separating ideas from specific instances
9. Interview question: Model up for us "chair entity" 
   --> [[Domain Driven Design#^328415]]
   what chair. It is as with maps, should i model the road map or terrain map etc.
10. Equals and hashcode contract - 
	The most popular general use collections are hash collections, HashSet and HashMap. 
    It is because it can give very very efficient operations, adding, removing, searching. Mechanism that let us do all of these operation very fast is called Hashing. It's very fast but it has a drawback. There can be a hash collision and the two different results can return the same hash, it happens very rarely but it can happen. That's why we should always has some help ready. And that is .equals() which we can then invoke to check if the object we get is 100% the obejct we want. If the equals for two object is true then their hashcode must be identical. Hashcode is identification on 99% and equals is 100% method. But we want to use Hashcode because of its speed.
11. Access modifiers
    - Public - it is accessible from any other class in your application
    - Protected - it allows for access to class with default access modifier within the same package and by subclasses in other packages
    - Package-scope (default) - allowed only within the same package
    - Private - allowed only from within the same class
	- Important that top-level class can be only public or default
	- ![[Pasted image 20240530194329.png]]
12. Final keyword
    - set limitation for extensibility
    - in Classes:
	    - When something is final it means that we cannot extend that, but i.e the object can be changed. It just cannot be extended
	    - With final keyword we lose extensibility
    - in Methods:
	    - Sometimes we don't need to set to final all class, we just need to declare a method final. Therefore this method cannot be overriden, thus extended
	- In variables: 
		- Variables marked as final can't be reasigned. Once a final variable is initialized. it can't be altered.
13. Static keyword 
    The static keyword in Java is used mostly for memory management. It goes with variables, methods, blocks, and nested classes.
    - static variable - there is only one copy of static variable per class, no matter how many objects are created (stored in static memory, not heap)
    - static method - you can call static method without creating instance of a class. static methods can only directly call other static methods or static variables.
    - static block
    - static nested class
14. Primitive types vs Object types
	- SEE 3.
	- In java there are two types of data types:
		- primitives: byte, short, int, long, float, double, boolean, char
		- they're passed by value, meaning that they're used as method argument, a new copy of the value is created
		- they have default values. i.e int= 0, boolean=false
		- object/reference data types:
		- object types are actual objects in memory, they have methods and fields
		- they're passed by reference meaning that when they are used as a method argument, the reference to the object is passed. Not the object itself
15. Types of collections 
	- Collection - root interface in the hierarchy of Java Collections. (it declares basics like: add(), remove(), clear(), isEmpty(), size())
	- List - extends Collection, represents Ordered Collection(a sequence). List can contain duplicate elements. user has control over where in the list each element are inserted (examples: ArrayList, LinkedList, Vector)
		- ArrayList - resizable array. Fast access, slow insertions and removals
		- LinkedList - slow access, fast insertions and removals
	- Set - extends Collection, represent a collection does not contains duplicates nor has any order (examples: HashSet, LinkedHashSet, TreeSet)
		- HashSet - hash table implementation of set, it doesn't maintain any order and allows null values
		- LinkedHashSet - hash table + linked list. It maintains insertions order
		- TreeSet - the elements are orders using heir natural ordering, or by a comparator provided at set creation time
	- Map - does not extend Collection. It represents an object that maps keys to values. Map cannot contain duplicate keys, each key can map to only one value (examples: HashMap, LinkedHashMap, TreeMap, HashTable)
		- HashMap - hash table implementation of map, it doesn't mantain any order and allows null for both key and values
		- LinkedHashMap: this is hash table + linked list implementation of map, with predictable iteration order
	- Queue - extends collection 
16. How does internet work?
    - This is a system of inter connected devices that communicate with each other using a set of protocols know as TCP/IP
    - When you connectto the internet your computer joins networrk, allowing it to communicate with other computers, this connections is usually facilliated by an ISP (Internet Service Privider)
    - Domain Name System (DNS) When you type an url into your browser. DNS dooing lookup. DNS translates the human readable web address into an IP address
    - HTTP reqquest, the browser can send the request with specified method (GET,POST, itp)
    - server processing
    - http response
17. Immutable - what means
    - immutable object is one wose state cannot be changed after it is created. Once a constructor for an immutable object completes the object is guaranteed to remain constant. this can be very helpfull in concurrent applications. Immutables are thread safe\
	    - all properties are final and private
	    - there are no setter methods
	    - if a field is a mutable object, then it must be defensively copied when passed between the class and its caller.
	    - String, Integer, Double, Character, Boolean are immutables
18. Java is being compiled into what
	- java source code is compiled into bytecode. Bytecode is platform independent representation of the source code that can be executed by JVM
	- It can be compiled into JAR (Java ARchive) or WAR(Web Application aRchive) file for distribution
	- Spring boot is flexible and app can be compiled both to JAR and WAR
19. Does primitive types has methods?
    - No they do not!
20. Polymorphism vs inheritance
    - Inheritance is mechanism that allows one class to acquire the fields and methods of another class. Acquired fields and methods are know as superclass. Inheritance is used for code reusability. Java does not support multi-inheritance
    - Polymorphism is mechanism to allow objects to take on may forms. Parent class can reference 
    - --------------------
21. How can we compare variables in java
    - primitives might be compared with relational operators (=, !=, >, <)
    - objects should be compared using equals() method to check if they are equal, this method should be overriden in the classes for correct comparisons
    - Objects.equals() method can be used which is null safe
    - comparing two optionals also would be null safe
    - apacheCommons ObjectiveUtils.equals()
22. After what class every other class in Java inherit?
    - in Java every class inherit after Object class 
    - it provides several methods that are universal to all objects, such as equals(), hashCode(), getClass(), toString()



* jak działa tworzenie stringa 
* warto wiedzieć jak java zarządza pamięcia i np poczytać o StringPool 
* string buffer vs string builder 

* roznice pomiedzy interface a klasa abstrakcyjna
* exceptiony 
* JVM 

* co to generyki 
* coś tam poczytaj o wątkąch 
* o SQL pytali 
* zebym z głowy złożyc 
* warto wiedzieć o clean code 
* o tym że np regexa gdzies definiować żeby komputer nie musiał go liczyć kilka razy 
* co to są referencje 
* teraz SPRING: 
* scopy beanów 
* co to są beany 
* co to jest maven 
* co to jest spring i jakie ma moduły 
* wyjaśnić działanie springa i tworzenie beanów 
* czy spring zawsze tworzy singletony 
* lazy loadig 
* relacje 
* co to kopozycja 

* Protokół HTTP ogarnąc jak działa

23. How does the garbage collectors work in Java? 
	- In Java JVM manages the memory by garbage collection. This process identifies and kills not used objects
	------------_EXPAND ON THE TOPIC----------
10. GO FURTHER WITH this https://github.com/Devinterview-io/java-interview-questions

3. Czy mozna zrobic metode prywatna w interfejsach i roznice miedzy interfejsami i klasami abstrakcyjnymi
2. Czy mozna wywolac metode statyczna na obiekcie
3. Czym sie roznic klasa od obiektu
4. Problem synchronizacji sesji użytkowników w n węzłach(skalowanie horyzontalne) za load balancer, Sticky sessions. 
5. Optimistic Locking, Pessimistic Locking 
6. Klucz główny, klucz obcy, zastosowanie indeksów(przyspieszenie wyszukiwania, unikalna niepowtarzalna wartość), rodzaje relacji, JPA a HIBERNATE jaka relacja 
7. git sposób pracy dla nowego projektu, merge, rebase, cerry pick 
8. przesyłanie plików z formularza, jakiego rozwiązania użyć 
9. sesja użytkownika, jak przebiega logowanie, różnica między autoryzacją a uwierzytelnianiem, jwt, oauth, itp. 
10. interfejs a klasa abstrakcyjna. Kiedy którego używać. Kompozycja a dziedziczenie. Wzorce projektowe. 
11. Komunikacja synchroniczna i asynchroniczna. 
12. Czym jest rest, z jakiego protokołu korzysta, jak używać metod z http 
13. Jaką architekturę wybrał byś dla nowej aplikacji. Monolit czy mikroserwisy, jakie pytania byś zadał przed rozpoczęciem prac 
14. testy jednostkowe a integracyjne 
15. Problemy z Lombok w połączeniu z hibernate 
16. Hibernate mapowanie hierarchii klas na tabele  
17. Propagacja i izolacja transakcji w Springu 
18. Przetwarzanie asynchroniczne zadań i semafory 
19. 16, Transactional parametry, stosowany wzorzec, jakie warunki muszą być spełnione 
20. co dodał spring boot do springa, spring.factories 
21. query plan 
22. eager and lazy loading, jaki wzorzec jest wykorzystany 
23. lombok jak zbudować bean z zależnością do innego beana 
24. postacie normalne, ACID, poziomy izolacji 
25. JPA locking 
26. Co z biznesowego punktu widzenia może się wydarzyć, kiedy przetwarzamy dane wsadowo i na koniec wysyłamy powiadamianie i uruchomimy kilka instancji aplikacji (zwielokrotnione przetwarzanie i powiadamiania) 
27. JMS, różnica między topic a queue 
28. SOLID, ACID, CAP 
29. Na czym polega skalowanie horyzontalne i wertykalne 
30. Mikroserwisy saga, punkt wejściowy do systemu(api gateway, backend for frontends)






1. 1. czym jest fasada 
2. czym się rózni Having od Where 
3. 3 czym jest cashowanie np przy pomocy redis 
4. 4 jak napisać sigletona odpornego na wielowątkowości 
5. 5 indeksy w sql czym sie różnia od siebie



1. Jakie mamy kolekcje w Javie
2. Różnica pomiędzy Setem a Listą
3. Kontrakt pomiędzy equals i hashode
4. Czym jest niemutowalność i jak stworzyć niemutowalną klasę
5. Porównanie wydajności ConcurrentHashMap i Collections.synchronizedMap

Hibernate:

1. Lazy vs Eager loading
2. Problem N+1
3. Adnotacja @Version

SQL:

1. Czym jest Primary Key
2. Typy joinów



- Co to jest CQS?
- Masz studentów i kursy - studenci mogą uczęszczać na wiele kursów, a na kursie może być wielu studentów - jak to by wyglądało w SQLu (jakie tabelki, jak połączone)?
- Jakbyś wykonał zapytanie do takich połączonych tabel.
- Kiedy powinno wybrać się bazę SQL, a kiedy noSQL?
- Co to jest refleksja? Gdzie jest wykorzystywana w Springu?
- Co robi słówko kluczowe 'transient' w javie?
- Jakie problemy z wydajnością może mieć Hibernate? (ewidentnie pytanie w kierunku problemu n+1) + na czym polega problem n+1?
- Czy kojarzę DDD? Z jaką architekturą się kojarzy? + o co chodzi w architekturze heksagonalnej - tutaj dodatkowe piwo dla każdej osoby, która wspominała o "Get your hands dirty..." bo byłem w trakcie czytania
- Czym są fail-fast i fail-safe?
- Zasady SOLID - czym są i skrótowo omówić




1. Czym jest enkapsulacja?
2. Czym są słowa kluczowe?
3. Jak działa Spring (a dokładniej jego kontener IoC)?
4. Czym jest i do czego służy klasa Locale (oraz inne powiązane klasy)?
5. Jakimi sposobami można zapisać liczbę w języku Java (podając ją w kodzie)?
6. Co to jest polimorfizm?
7. Jaka jest różnica między listą i setem?
8. Czym się różni webservice typu REST od webservice typu SOAP?
9. Czym jest, i jak działa Garbage Collector w Javie?
10. Jaka jest różnica pomiędzy klasami String oraz StringBuilder/StringBuffer?
11. Jakie typy numeryczne są dostępne w Javie?
12. Kiedy należy używać podejścia iteracyjnego?
13. Jaka jest różnica pomiędzy klasą abstrakcyjną a interfejsem?
14. Czym są mikroserwisy i kiedy warto je stosować?
15. Jakie typy baz danych wyróżniamy?
16. Co to znaczy, że obiekt jest niemutowalny?
17. Kiedy należy używać podejścia rekursywnego?
18. Na czym polega wzorzec projektowy Builder (budowniczy)?
19. Czym się różni wzorzec proxy od adaptera?
20. Czym jest ‘bean’ w Springu, co może, a co nie może nim być?
21. Jakie są scope'y beanów?
22. Na czym polega kontrakt equals-hashCode?
23. Czym jest i do czego służy interfejs java.lang.Comparable?
24. Czym jest i jak działa JDBC?
25. Co to jest servlet?
26. Czym się różni wyjątek typu checked od unchecked?
27. Co to jest i do czego służy Hibernate?
28. Co powoduje zastosowanie słowa kluczowego final?
29. Czym w Javie różni się final od finally i finalize?
30. Co to jest JIRA?
31. Czym jest Spring. Zalety Springa?
32. Na czym polega technika TDD?
33. Czym są wzorce projektowe?
34. Czym jest GIT?
35. Jaka jest różnica pomiędzy equals() a „==”?
36. Czym jest JVM (maszyna wirtualna Javy)?
37. Co to są streamy?
38. Co to są Lambdy?
39. Czy w obiekcie klasy String można przechowywać dane wrażliwe? Dlaczego?
40. Co to jest JDK?
41. Czym jest enum?
42. Czym jest deadlock?
43. Czym jest race condition?
44. Rodzaje testów?
45. Czym jest lazy loading?
46. N+1 problem w Hibernate?
47. Co to jest Kafka?
48. SQL Having vs Where?
49. Czy wiesz co umożliwia JDBC?
50. Czym jest ORM? Jaki jest najbardziej rozbudowany / popularny ORM W Javie?
51. Jaka jest różnica między jpa, Spring Data JPA, a hiberante?
52. Założenia SOLID?
53. Co to jest Left Outer JOIN (SQL)?
54. Co to jest Inversion of Control?
55. Co to jest String?
56. Co to jest Web Service I jakie są rodzaje?
57. Na czym polega wielowątkowość?
58. Jaka jest różnica między wątkiem, a procesem w Javie?
59. Czym jest debugowanie?
60. Co oznacza dla Ciebie czysty kod?
61. Jakie są różnice między SVN a GIT?
62. Czy może wystąpić wyciek pamięci w Garbage Collector? Do czego się go używa?
63. Jakie znasz kolekcje w Javie?
64. Różnica między List, ArrayList, Linked List, Vector.
65. Różnica pomiędzy HashSet vs HashMap?
66. Złożoność obliczeniowa HashMap, metody get na HashMapie?
67. Equals() vs hashCode();
68. Co oznacza słowo kluczowe final?
69. Co oznacza, że klasa jest niemutowalna?
70. Jakie znasz rodzaje wyjątków?
71. Wzorce projektowe?
72. Co to jest interfejs funkcyjny?
73. Co to jest klasa anonimowa?
74. Co to jest Optional?
75. Co to jest Docker?
76. Co to jest kod maszynowy?
77. Wyjaśnij co to jest i do czego służy XML.
78. Kiedy dane trafiają zamiast do string poola to od razu trafiają do pamięci lokalnej.
79. Jaki typ zmiennych użyłbyś do operacji bankowych?
80. HashMapa - co wiesz i od razu pytanie co się dzieje gdy wywołasz metodę .put()?
81. Jak działa garbage collector? Jakie znasz implementacje GC?
82. Co oznacza adnotacja @Transactional?
83. Opisz cykl życia encji.
84. Co to jest profiler i do czego się go wykorzystuje?
85. Co to jest protokół HTTP?
86. Co to jest REST API?
87. Co to jest programowanie aspektowe.
88. Spring vs Quarkus, czy są różnice w wydajności?
89. Z czym związany jest problem w SQL (n+1)?
90. Zapytania REST w Springu (GET, PUT itd)?
91. Co to jest idempotentność?
92. W jaki sposób przekazywane są dane z przeglądarki do kontrolera?
93. Opisz Gitflow.
94. Spring Cloud - co to jest?
95. Testy jednostkowe i wszystkie adnotacje z @BeforeEach itd.
96. Kody odpowiedzi HTML.
97. Napisz SQL, który połączy dwie tabele.
98. Główna zaleta protokołu HTTP.
99. Jakie są żądania HTTP i jakie są grupy kodów odpowiedzi?
100. Co to jest mock?
101. Czym charakteryzuje się klasa AtomicInteger?
102. Co to jest indeksowanie w bazie danych i jakie są tego wady i zalety?
103. Co to jest Optional?
104. Co to jest Kubernetes? Jakie są jego cechy i do czego służy?
105. Jakie jest powiązanie Kubernates i Docker?
106. Zasady pracy zgodne z Agile.
107. Kto jest w zespole Agile?
108. Kto to jest Stakeholder?
109. Jakie spotkania są w Scrumie?
110. Co to jest Product Backlog?
111. Słowo kluczowe static.
112. Czym jest autoboxing i autounboxing w Javie?
113. Czy szybciej wykonują się operacje na typach prostych czy obiektowych?
114. Rodzaje kolekcji w Javie (po 2 zdania charakteryzujące).
115. Jak w łatwy i szybki sposób można usunąć duplikaty z Listy?
116. Jakimi jednostkami można definiować wysokość i szerokość w CSS?
117. Jaka jest różnica między jednostkami rem i em?
118. equals(), compareTo(), ==?
119. BigDecimal w liczbach, kwotach i bankowości? Co?
120. Rodzaje map, set, list?
121. Co to jest Continuous Integration?
122. Co to jest i jak działa ForEach, Iterator?
123. Po co jest hash w HashMapie?
124. Co to są mikroserwisy? Jak implementować wsdl, jak?
125. Dziedziczenie w Hibernate, JPA, jak działa?
126. Wyjątek lazy w Hibernate i jak go uniknąć?
127. Klasa Number?
128. Fullscan?
129. Co to jest XPath?
130. Private, public, protected - różnice?
131. Klasa w klasie - jak to działa?
132. Final, static?
133. SQL Injection?
134. Hibernate session vs sessionFactory?
135. Spring @Autowired byName, constructor, byMethod?
136. Hibernate session.get, session.load?



https://www.edureka.co/blog/interview-questions/java-interview-questions/