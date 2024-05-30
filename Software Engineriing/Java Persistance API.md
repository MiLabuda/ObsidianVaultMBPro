Basic Java framework for comunicating with database is JDBC (java database connection). It has a few cons, there is a lot of boilerplate when retrieving data from db tables and also there is a problem with portability because since we have to use querries for specific data base server ew are using, we need to use proper syntax. Syntax which is different between different db servers, other cons are i.e updating a data base table and all its relationships etc.

JPA exist to solve the problems of using bare jdbc. It solves the problem with mapping our java objects to database objects. It will map the class structure to the db tables etc.

“A group of specifications (a lot of texts, regularizations and Java Interfaces) to define how a JPA implementation should behave”. There are many JPA implementations available both free and paid, e.g. Hibernate, OpenJPA, EclipseLink

JPA created a db language named JPQL to perform db querries. The main advantage of this that it can be run in all databases


