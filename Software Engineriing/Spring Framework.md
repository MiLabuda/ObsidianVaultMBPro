
Framework is different from library because library is just a bunch of already written code that we can reuse, some helper methods(), Frameworks on the other hand provide some level of control. YOur codebase is under control of the framework.

We can call a framework if:
- Inversion of Control - 
  "Don't call us, we will call You" - that can describe inversion of control very well
we no longer have to take care of dependencies management because the framework like Spring will take care of this
- Default Behaviour - 
  When working with framework we do not have to set and configure everything, those parts that we do not configure will have a default behaviour
- Extensibility - 
  We as a framework users must be able to provide our logic to solve business problems, so we will get the predefined abstractions, but we will specialise it by ourselves
- Non-modifiable framework code
  Frameworks has well structured abstractions that already are able to solve its purposes very well, but also need to be non-modifiable by developers to stay consist.

Bean in Spring - In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.

Component - fully assembled bean and managed by the container

Container - assembler and manager of components. i. e. Docker and Spring are popular containers. they can also perform validation for beans, enforce security constraints, manage transactions for backend etc.

 [[Docker]] - supplies a container that assembles and packages software so that it can be generically executed on remote platforms.