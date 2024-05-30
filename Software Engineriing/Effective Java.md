


# 2 Chapter: Creating and destroying objects 
## 1  Consider static factory methods instead of constructors
dont understand it much right now

## 2 Consider a builder when faced with many constructor parameters

Static method is created when the method belongs to the class itself and doesn't need any object. Can be called without any of the class instances.
Non static method belongs to the objects of that class

Static method can only access static data members and static methods of another class or the same class but cannot access any non static methods and variables

Java bean is a:
- all properties are private (using getters/setters)
- a public no-argument constructor is available
- implements serializable

Better solution for making objects than with JavaBeans is to create Builder, the obligatory fields set as final and the rest set to default values and set up Builders for each with field name.

The builder design pattern should be created for classes that has 4 fields or more. 
The disadvantage is that its creation costs, that not an issue in normal application but in critical system it is worth to notice.

## 3 Enforce the singleton property with a private constructor or an enum type

Singleton - class that is instantiated exactly once.

Best approach is to implement singleton as a single element ENUM type

## 4  Enforce noninstantiability with a private constructor

static methods are against OOP concept but its sometimes used for writing utility classes. Such classes can't be instantiated.

To enforce noninstantiability we can suppress default constructor with private constructor i.e
```
private UtilityClass() {
throw new AssertionError()
}
```

Such class cannot also be subclasses, because all subclasses need to invoke superclass constructors

## 5 Prefer dependency injection to hard-wiring resources
Simplest way for dependency injection is to pass the resource into the constructor when creating a new instance.

Dependency injection - in short it is a way to put outside all our dependencies to not have them hardcoded in class or created in our classed but provided from the outside

Injecting dependencies via constructor we can also provide the factory method, and such factory can be the ```Supplier<T> ``` Interface 

## 6 Avoid creating unnecessary objects
We should always aim in reusing objects instead of creating new ones
AVOID something like this:
```
String s = new String("bikini"); ////DONT DO THIS!!
```
This statement creates new String instance each time it is executed, if such code run very often this can lead to needless creation of huge amount of objects. Instead when doing
```
String s = "bikini"
```
we use something like String pool, in this situation we are sure that the object will be reused by many instances.
String pool: **the special memory region where _Strings_ are stored by the JVM**.

Strings in JAva are immutable. Thanks to that JVM can optimize the memory for them by storing only of copy of each literal String in the pool. This process is called interning.
When we create a String variable and assign a value to it, the JVM searches the pool for a String of equal. if it finds one it will just return a reference to the memory address instead of allocating additional memory. If it won't find any it will add it to the String pool


If we will create String by the constructor it will create a new object and store it in the heap space reserved for the JVM. Every string created like this will point to different memory address

more info: [[https://www.baeldung.com/java-string-pool]]

One of the way of creating unnecessary objects is often auto-boxing, (wrapping primitive types with its reference types int -> Integer, unboxing means Integer -> int)

## 7 Eliminate obsolete object references
Memory leaks in programing languages that has garbage collection are called, uintentional object retentions

This chapter says a lot about helping garbage collection in rare cases, but it won't bother me much so i skip noting this.

Interesting things that cache are often source of memory leaks. When they stay in the cache long after they're truly needed

## 8 Avoid finalizers and cleaners
“Finalizers are unpredictable, often dangerous, and generally unnecessary.”

A lot of sources about closing objects with finalizers and cleaners by myself. But since it'd quite hard topic I will leave it for now. It bores me to death

## 9 Prefer try-with-resources to try-finally

Historically the best way to be sure to close all the resources was try-finally, but now the try-with-resources is best option.

the resource for try-with-resource must implement AutoClosable, to if we implement it by ourself we must remember that.




# 3 Chapter: Methods common to all objects
## 10 Obey the general contract when overriding equals
Object is a concrete class but it's designed for extension, therefore all non-final methods (equals, hashCode, toString, clone) have general contracts because they are designed to be overriden. If we fail to do meet these contracts it will prevent the other classes which depend on the contracts (like HashMap, HashSet) from functioning properly

The easiest way to avoid problem during overriding equals is to not to do that.
Then the object of that class would be equal only to itself. 
To do this we need to meet this criteria:
- Each instance of the class is permamently unique
- there is no need for the class to provide a logical equality test
- a supercalss has already overridden equals, and the superclass behaviour is appropriate for this class (for example most SET implementations, inherit their equals implementaation from AbstractSet, and same with List and Map)
- the class is private or package-private and you are certain that its equals method will never be invoked


equals() should be implemented when a class has a logical equality and a superclass has not already overriden equals, such method should checkw wheter the objects of that class are logically equivalent, not if they refer to the same object

The equals method implement an equivalence relation, it has these properties:
- Reflexive
- Symmetric
- Transitive
- Consistent.
(to do, best watch YT video about it)
When you finish writing an equals method ask yourself if it is symmetric, transitive, consistent?

GOOD PRACTICEs with implementing equals:
- Use the == operator to check if the argument is a reference to this object(only a performance optimization, but for expensive comparison it's worth doing)
- Use the instanceOf operator to check if the argument has correct Type
- Cast the argument to the correct type 
- check if that field of argument matches the corresponding field of this object for each 'significant' field in the class

 equals argument should always be of type (Object o) and we should always remember to Override than Overload.
 We can use IDE generated implementation of equals and hashcode


## 11 Always override hashCode when overriding equals

## 12 Always override toString()
# 4 Chapter
## 15 Minimize the accessibility of classes and members

Encapsulation is a hiding the internal implementation and comunitacing betwen modules via API, 

GOOD PRACTICE is to make each classes or members as inaccessible as possible

The line between public API and internal implementation lies between public and package-private, package-private (default) is already part of the implementation and we should be able to change the implementation whevever we want.

It's good to think when we have a class that is used only by some other class, we can make the original class of private static member of the second class to limit the access to all other classes

## 16 In public classes, use accessor methods, not public fields

Encapsulation should be used for public and protected classes, but there is no wrong to use plain values in private and package-private methods.

public classes should NEVER expose mutable fields

## 17 Minimize mutability

to make a class immutable you should
- don't provide methods that modify the object's state (mutators)
- Ensure that class can't be extended (private constructors!, making class final! public static factories instead constructors)
- make all fields final
- make all fields private
- ensure exclusive access to any mutable components(make defensive copies)

functional approach is a pattern where method returns the result of applying functions to their operand, without modyfing it.

Immutable objects are also Thread-safe, they do not require any synchronization.

Using static factories for creating new objects, can also give the opportunity for implementing caching i.e the most often used objects and therefore minimize the memory usage footprint and garbage collection footprint

Immutable objects make great builduing block for other objects

Immutable objects provide failure atomicity for free [##76]

## 18 Favor composition over inheritance



# 5 Chapter
# 6 Chapter
# 7 Chapter: Lambdas and streams

## 42 Prefer lambdas to anonymous classes
This chapter shortly describes the lambdas and how it is a step up from the interfaces before. Now hey're called functional interfaces. 

Lambdas works best when short. Ideal is 1 line, but 3 is max. Longer lambdas are clear sign that you should refactor your code.

Lambdas are prefered to anonymous classes, therefore more often used nowadays

## 43 Prefer method references to lambdas
Code like this:
```
map.merge(key, 1, Integer::sum);
```
is a great example of using method references, for the map if the key exists add 1 to the value if no then add key with value 1

Everything you can do with method reference you can do with lambdas
They're mainly used for shorter clearer code

Where method references are shorter and clearer, use them; where they aren’t, stick with lambdas.


## 44 Favour the use of standard functional interfaces
Try to always use the standard methods from native java library or standard functional interfaces to make your code easier to understand.

Java lib java.util.Function consist of 43 interfaces. There are 6 basic of them:
```
- UnaryOperator<T>     --- T apply(T t)        --- String::toLowerCase
- Binary Operator<T>   --- T apply(T t1, T t2) --- BigInteger::add
- Predicate<T>         --- boolean test(T t)  --- (Collection::isEmpty)
- Function<T, R>       --- R apply(T t)       --- Arrays::asList
- Supplier<T>          --- T get()            --- Instant::now
- Consumer<T>          --- void accept(T t)    --- System.out::println
```


Operator interfaces represents a function that takes the argument and return the result of the same type.

Predicate is on the other hand a function that takes an argument and return boolean

Function interface represent a function that takes the argument and return the result of the different type. Argument and result differs.

Supplier interface represents a function that takes no value and return result in other words supplies the value

Consumer is a interface that represents a function that takes an argument and return nothing. Just consuming its argument.

For each of these functional interfaces there are 3 variants for primitive types like int, long, double.

There are a lot of functional interfaces above these basic 6 that provide special handling for primitive types. GOOD PRACTICE. Dont use basic functional interfaces and put Boxxed primitives there, it can be deadly for performance and its non logical. You should use dedicated functional interfaces since there are created specially for such ocassions. "Prefer primitives to boxed primitives" 

Sometimes writing your own functional interface is better than using basic ones i.e 
```Comparator<T>```

You can write your own if these conditions are met:
It will be commonly used and could benefit from a descriptive name.
It has a strong contract associated with it.
It would benefit from custom default methods.

While implementing your own functional interface you should remember about annotation  @FunctionalInterface



## 45 Use streams judiciously
Streams are simply finite or infinite sequence of data elements
Stream pipelines are multistage computation on those elements

Stream pipeline consists of a source stream followed by 0 or more intermediate operations finalized with 1 terminal (final) operation.

Stream's operations are evaluated lazily that means, that any intermediate operation waits until it's needed so it waits for the terminal operation and then is computed.

Using streams in place of every loop in our program is NOT the best idea, we should have in mind that on the first place is clear code.

From a codeblock in comparison with streams, lambdas or method references (function objects) you can read or modify any local variable in scope; from a lambda you can only read final or effective final variables. and you cant modify any local variables.
From a code block you can return from the enclosin method, break or continue and enclosing loop, or throw any checked exception.

flatMap operation maps each element in a stream to stream, and next contatenates all the streams to the one stream (flattens them) 


## 46 Prefer side-effect free functions in streams

The forEach terminal operation on stream should only be used to report the result, never to perform a computation. Developers might be tempted to use this for computation because that's the tool theyre familiar with but it a bad practise.

Collectors API used with streams are responsible for reducing output of the stream into one single object which typically is a collection.

“It is customary and wise to statically import all members of Collectors because it makes stream pipelines more readable.” --- I don't understand it yet
---> import static java.util.stream.Collectors.*; static import looks like this and can shorten the calls to the Collectors API

"downstream collectors" - In practice, when using methods like `groupingBy` or `partitioningBy` from the `Collectors` class, you can provide another collector as an argument that operates on the results of the previous collector. This second collector, which acts on the results of the first one, is called the "downstream collector."
```
Map<Integer, Long> wordLengthCount = words.stream() .collect(Collectors.groupingBy(String::length, Collectors.counting()));
```

“In summary, the essence of programming stream pipelines is side-effect-free function objects. This applies to all of the many function objects passed to streams and related objects.”

## 47 Prefer Collection to Stream as return type

In summary, when writing a method that returns a sequence of elements, remember that some of your users may want to process them as a stream while others may want to iterate over them. Try to accommodate both groups. If it’s feasible to return a collection, do so.

This chapter was very abstract to me i didn't really encounter the problem related to that subject yet, i didn't understand much, therefore i didn't do much notes, maybe will comeback to that later

## 48 Use caution when making streams parallel

Streams can be parallelized with .parallel() method

parallelizing a pipeline is unlikely to increase its performance if the source is from Stream.iterate, or the intermediate operation limit is used

Performance gains from parallelizm are best on streams over ArrayList, HashMap, HashSet, ConcurrentHashMap instances, int ranges, long ranges, because they can be easily divided into smaller ranges what makes easy dividing work between threads while parallelizing

IN some specific cases when doing work on BIG collections of data while streaming them it can be useful to use parallel() to add additional threads for doing the work.
Unfortunatelly it is rarelly the case and its better to be very careful when using parallel

# 8 Chapter: Methods
##  49 Item: Check parameters for viability
failure to validate parameters can result in violation of "failure atomicity". We must detect the errors from the parameters as soon as possible

it says that we should use the javadoc to describe possible errors thrown bny violating our api

for checkining for null we can use Objects.requireNonNull method

At non public methods, these that we are not exporting to the world we can use assertions in the code to ensure valid parameters

```
assert a != null;
```

they throw assertion errors

It is also super important to validate fields before construction of an object

Sometimes when methods throws an error it is different error then the one that caused the issue, in that case we can use 'exception translation idiom' 73 Item[##73 Item]

## 50 Make defensive copies when needed

you should always programm deffensively because someone even with good intensions may broke your methods.

It is GOOD PRACTICE to make defensive copies in constructor, to exclude any chances of mutating states of given parameters.

```
“public Period(Date start, Date end) {
    this.start = new Date(start.getTime());
    this.end   = new Date(end.getTime());”
}
```
in this example Date in java library is mutable so it would be possible to create Period object with start,end values and after that these values could have been changed

Also GOOD PRACTICE is to make defensive copies BEFORE validating parameters, this prevents from time-of-check/time-of-use TOCTOU attack. from another thread during the window of vulnerability

Also GOOD PRACTICE is to avoid using .clone() on parameter objects whose type can be subcassable by untrusted parties

Also GOOD PRACTICE is to modify accessors method to return defensive copies of mutable internal fields. like so

```
public Date start() {
    return new Date(start.getTime());
}
```

Try to use immutable objects whenever possible to not think about defensive copies

Defensive copies may be omitted when we trust the caller class, i.e when it in the same package

## 51 Design method signatures carefully
Choose method names carefully!!

GOOD PRACTICE is to keep signature with less than 4 parameters. to achieve that you can: 
- break method into smaller pieces. 
- create helpers methods (ussually as a staticmember class)
- adapt the BuilderPattern 

GOOD PRACTICE is to use interfaces in signatures whenever possible

Prefer two-element enums to boolean parameters. Use bolleans only when it is 100 clear from the context to use it. Otherwise create enum type, its easier to read after, also you can add more options later

## 73 Item: 
aasgasgasgasga
# 9 Chapter
# 10 Chapter
# 11 Chapter
