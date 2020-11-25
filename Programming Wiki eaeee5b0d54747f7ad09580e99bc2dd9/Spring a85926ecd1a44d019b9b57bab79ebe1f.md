# Spring

# Abour Spring

## Spring Container

- Primary functions
    - Create and manage objects (Inversion of Control)
    - Inject object's dependencies (Dependency Injection)

## Spring Bean

- Spring Bean is a Java object which is instantiated, assembled and managed by Spring container.
- Spring Container will automatically instantiate @bean object.

## **Confinguring Spring Container - 3 ways**

- **XML configuration file** (legacy, but most legacy apps still use this)
- **Java Annotation** (modern)
- **Java Source Code** (modern)

## Why Using Interface.

Coding to an interface is a best practice in Java. Your code will still run if you just use classes instead of interfaces but there are a number of significant advantages to the practice:

1. Clients remained decoupled and unaware of the specific class they are using, as far as the underlying class adheres to the defined interface.
2. The client does not need to know with which concrete class its interacting.
3. Depending on the context, different implementation classes can be polymorphically provided without having to change client code. It makes it easy for the system to evolve.
4. During development even though the concrete class is not available, the client class can be developed. It helps parallelize work.

## Inversion of Control (IoC)

### Traditional Programming - Not using IoC

- Our custom code makes calls to a library
- traditionaly, we directly create a new object inside an object, which means we create the dependent object.

### Programming with IoC

- IoC enables a framework to take control of the flow of a program and make calls to our custom code.
- IoC has a container to help us to create those dependent objects and inject the dependent object to our codes. → this is an inversion
- "所有的类都会在spring容器中登记，告诉spring你是个什么东西，你需要什么东西，然后spring会在系统运行到适当的时候，**把你要的东西主动给你**，同时也**把你交给其他需要你的东西**。所有的**类的创建、销毁都由 spring来控制**，也就是说控制对象生存周期的不再是引用它的对象，而是spring。对于某个具体的对象而言，以前是它控制其他对象，现在是所有对象都被spring控制，所以这叫控制反转。"

### Benefits of Using IoC

- Decoupling the execution of a task from its implementation
- Making it easier to switch between different implementation
- greater modularity of a program
- greater ease in testing a program by isolating a component or mocking its dependencies and allowing components to communicate through contracts.

## Dependency Injection (DI)

- **dependency?** - The program depends on IoC container
- **why**? - The program needs IoC container to provide the resources for its objects
- **Injection**? - IoC container injects to the object.
- **what**? - The needed resources (including objects, resources, variables) of an object is injected.
- The act of connecting objects with other objects, or "injecting" objects into other objects, is done by an assembler(container) rather than by the objects themselves.

```java
// traditional programming
public class Store {
	private Item item;

	public Store() {
		item = new ItemImpl();
	}
}
```

```java
// By using DI
public class Store {
	private Item item;

	public Store(Item item) {
		this.item = item;
	}
}
```

### How Java Performs DI

- Using reflection to perform the process of injection

---

# 1. Spring Configuration - XML Configuration

## XML Configuration - Inversion of Control

### Spring Development Process

- Configure your Spring Beans

    File: applicationContext.xml

    full qualifeid class name of implementation class.

    ```xml
    <beans ...>
    	<bean id="myCoach" 
    		class="com.allen.springdemo.BaseballCoach"> // impl class
    	</bean>
    </beans>
    ```

- Create a Spring Container

    Spring contaniner  = ApplicationContext

    ```java
    // create a spring container
    ClassPathXmlApplicationContext context = 
    						new ClassPathXmlApplicationContext("theNameOfConfigFile.xml")
    ```

- Retrieve Beans from container

    ```java
    // create a spring container
    // .....

    // retrieve bean from spring container
    Coach theCoach = context.getBean("beanID", Coach.class); // interface class
    ```

## XML Configuration - Dependency Injection

### Development Process - **Constructor Injection**

1. Define the dependency interface and class. ( dependency = helper)
2. Create a constructor in your class for injections 
3. Configure the dependency injection in Spring config file.

create and inject 

![Spring%20a85926ecd1a44d019b9b57bab79ebe1f/Untitled.png](Spring%20a85926ecd1a44d019b9b57bab79ebe1f/Untitled.png)

---

![Spring%20a85926ecd1a44d019b9b57bab79ebe1f/Untitled%201.png](Spring%20a85926ecd1a44d019b9b57bab79ebe1f/Untitled%201.png)

---

### Development Process - **Setter Injection**

1. Create setter method(s) in your class for injections

    File: XxxxCoach.java

    ```java
    // Called by Spring during setter injection
    public void setFortuneService(FortuneService fortuneService) {
    	this.fortuneService = fortuneService;
    }
    ```

2.  Configure the dependency injection in Spring config file

File: applicationContext.xml

```xml
<bean id="myFortuneService"
	class="com.allen.springdemo.HappyFortuneService">
</bean>

<bean id="myCricketCoach"
	class="com.allen.springdemo.CricketCoach">

	<property name="fortuneService" ref="myFortuneService" />
</bean>
```

When processing, Spring will be looking for a public setter method in the class - setFortuneService (behind the scene)

[Additional Spring Bean Scopes](Spring%20a85926ecd1a44d019b9b57bab79ebe1f/Additional%20Spring%20Bean%20Scopes%20cb7fcd903cdf4de9a2f6ccb345e6582d.csv)

### **Singleton**

- spring container creates only one instance of the bean, by default
- it is cached in memory,
- all requests for the bean
    - will return a **SHARED** reference to the SAME bean - points to the same area in the memory.
- you can decalre the scope for beans

---

### **Prototype**

- Create different objects for each beans

    ```xml
    <bean id="myCoach"
    		class="com.allen.springdemo.TrackCoach"
    		scope="prototype">
    ```

- Load the applicationContext.xml file in the App.

    ```java
    // load the spring configuration file
    ClassPathXmlApplicationContext context = 
    				new ClassPathXmlApplicationContext("beanScope-applicationContext.xml");
    ```

---

### **Bean Lifecycle**

Container Started → Bean Instantiated → Dependencies Injected → Internal Spring Processing → Your Custom Init Method → Bean Is Ready For Use, Container Is Shutdown → Your Custom Destroy Method → STOP

- we can add custom code during **bean initialization**
    - Calling custom business logic methods
    - setting up handles to resources (db, sockets, file etc)
- we can add custom code during **bean destruction**
    - calling custom business logic methods
    - setting up handles to resources (db, sockets, files etc)

- We can define the bean lifecycle method in the applicationContext.xml file.
    - init-method : setup bean init
    - destroy-method : setup bean destroy

    ```xml
    <bean id="myCoach"
    		class="com.allen.springdemo.TrackCoach"
    		init-method="doMyStartupStuff"
    		destroy-method="doMyStartupStuff">
    ```

- Load the applicationContext.xml file in the App.

---

- **Defining init and destroy methods - Method Signatures**

    **Access modifier** 

    - can have any access modifier(public, protected, private)

    **Return type** 

    - can have any return type. "void" is most commonly used
    - If you give a return type just note that you will not be able to capture the return value.

    **Method name** 

    - Any

    **Arguments** 

    - cannot accept any arguments, should be no-arg

### **Special Note about Destroy Lifecycle and Prototype Scope**

- For "prototype" scoped beans, Spring does not call the destroy method.
- Spring does not manage the complete lifecycle of a prototype bean: the container instantiates, configures, and otherwise assembles a prototype object, and hands it to the client, *with no further record of that prototype instance.*
- in the case of prototypes, configured destruction lifecycle callbacks are not called. **The client code must clean up prototype-scoped objects and release expensive resources that the prototype bean(s) are holding.**

### **Development Process - call the destroy method on prototype scope beans**

1. Create a custom bean processor. This bean processor will keep track of prototype scoped beans. During shutdown it will call the destroy() method on the prototype scoped beans. The custom processor is configured in the spring config file.

    ```xml
    <!-- Bean custom processor to handle calling destroy methods on prototype scoped beans --> 
    <bean id="customProcessor"	
    	class="com.luv2code.springdemo.MyCustomBeanProcessor">	
    </bean>
    ```

2. The prototype scoped beans MUST implement the DisposableBean interface. This interface defines a "destory()" method.

    ```java
    public class TrackCoach implements Coach, DisposableBean { 

    	...

    	// add a destroy method	
    	@Override	
    	public void destroy() throws Exception {	
    		System.out.println("TrackCoach: inside method doMyCleanupStuffYoYo");		
    	} 
    }
    ```

3. The spring configuration must be updated to use the destroy-method of "destroy".

    ```xml
    <bean id="myCoach"	
    	class="com.luv2code.springdemo.TrackCoach"	
    	init-method="doMyStartupStuff"	
    	destroy-method="destroy"	
    	scope="prototype">			

    	<!-- set up constructor injection -->	
    	<constructor-arg ref="myFortuneService" />	
    </bean>
    ```

4. In this app, BeanLifeCycleDemoApp.java is the main program. TrackCoach.java is the prototype scoped bean. TrackCoach implements the DisposableBean interface and provides the destroy() method. The custom bean processing is handled in the MyCustomBeanProcessor class.

---

# 2. Spring Configuration with Java Annotation

## Java Annotation - Inversion of Control

### Bean Names

- using "@Component" to get beans, so the bean id by default would be the "className"
- using "@Component("beanId")" , the bean id will be the "beanId".
- **the special case**: if the first and the second letters of the class name are uppercase, name is not converted.

    e.g. XMLService → XMLService.

    ```java
    @Component("tennisCoach")
    public class TennisCoach implements Coach {
    	...
    }
    ```

- Specify in the applicationContext.xml to setup the component scanning

    ```xml
    <!-- add entry to enbale component scanning -->
    	
    <context:component-scan base-package="com.allen.springdemo" />
    ```

### Spring Container

- in the App class:

    ```java
    // read spring config file
    ClassPathXmlApplicationContext context = 
    					new ClassPathXmlApplicationContext("applicationContext.xml");

    // get the bean from spring container
    Coach theCoach = context.getBean("tennisCoach", Coach.class); // bean id

    // call a method on the bean
    System.out.println(theCoach.getDailyWorkout());

    // close the context
    context.close();
    ```

## Java Annotation - Dependency Injection

### Dependency Injection - SpringAutoWiring

- **Spring will look for the class (has @Component) which implement the matched interface, and use it to inject.**
    - Choose the injection type consistently throughout the project. (xml, java with xml, pure java)

### Development Process - Constructor Injection

- Using "@Autowired" annotation for constructor injection
    - **Constructor Injection**
        - In the example lecture, Spring will scan for a component that implements the interface 'FortuneService' - that's 'HappyFortuneService'
            - Inject the happyFortuneService into the object when Spring instantiates a new object of TennisCoach.
        - That's the reason why it need "@Component" in the HappyFortuneService class.

        File: HappyFortuneService.java

        ```java
        @Component
        public class HappyFortuneService implements FortuneService {

        	@Override
        	public String getFortune() {
        		
        		return "Today is your lucky day";
        	}

        }
        ```

        File: TennisCoach.java

        ```java
        @Component
        public class TennisCoach implements Coach {

        	private FortuneService fortuneService;
        	
        	@Autowired
        	public TennisCoach(FortuneService theFortuneService) {
        		fortuneService = theFortuneService;
        	}
        	
        	...

        }
        ```

### Development Process - Setter (Method) Injection

- **Setter(Method) Injection** - Development Process
    1. Create setter method(s) in your class for injections
    2. Configure the dependency injection with **@Autowired** Annotation
        - Note: it can be any method

    File: TennisCoach.java

    ```java
    	@Autowired
    	public void setFortuneService(FortuneService fortuneService) {
    		this.fortuneService = fortuneService;
    	}

    	// The other way - in any methods.
    	@Autowired
    	public void doSomething(FortuneService fortuneService) {
    		System.out.println(">> TennisCoach: inside doSomething method");
    		this.fortuneService = fortuneService;
    	}
    ```

### Development Process - Field Injection

- **Field Injection**
    1. Configure the dependency injection with Autowired Annotation
        - Applied directly to the field
        - No need for setter methods.

    ```java
    @Component
    public class TennisCoach implements Coach {

    	@Autowired
    	@Qualifier("randomFortuneService")
    	private FortuneService fortuneService;
    	
    	// define a default constructor
    	public TennisCoach() {
    		System.out.println(">> TennisCoach: inside default constructor");
    	}
    ```

### Dependency Injection with Multiple Implementations

FortuneService interface has mutiple implementations.

Solution: @Qualifer → to be specific

- can apply to any injection
- **special case while using with constructor injection**

    ```java
    @Autowired
    public TennisCoach(@Qualifier("randomFortuneService") FortuneService theFortuneService) {
    	System.out.println(">> TennisCoach: inside constructor using @autowired and @qualifier");
      fortuneService = theFortuneService;
    }
    ```

> How to inject properties file using Java annotations

1. Create a properties file to hold your properties. It will be a name value pair.  

New text file:  src/sport.properties
`foo.email=myeasycoach@luv2code.com
 foo.team=Silly Java Coders`
Note the location of the properties file is very important. It must be stored in src/sport.properties

2. Load the properties file in the XML config file.
File: applicationContext.xml
Add the following lines:

    `<context:property-placeholder location="classpath:sport.properties"/>`  

This should appear just after the <context:component-scan .../> line

3. Inject the properties values into your Swim Coach: SwimCoach.java

`@Value("${foo.email}")
 private String email;

 @Value("${foo.team}")
 private String team;`

### Bean Scope for Annotations - singleton & prototype

1. Explicitly Specify Bean Scope
    - Singleton Scope

        Using "@Scope("singleton") for the Class

    - Prototype Scope

        New object for each request

        @Scope("prototype")

### Bean Lifecycle for Annotations - init and destroy methods

1. Define your methods for init and destroy
2. Add annotations: @PostConstruct and @PreDestroy
    - @PostConstruct → Code will execute after constructor and after injection of dependencies
    - @PreDestroy → Code will execute before bean is destroyed.
        - **CARE**! Not called by prototype scoped bean.

        can destroy prototype beans but custom coding is required. This examples shows how to destroy prototype scoped beans.

        1. Create a custom bean processor. This bean processor will keep track of prototype scoped beans. During shutdown it will call the destroy() method on the prototype scoped beans.

        2. The prototype scoped beans MUST implement the DisposableBean interface. This interface defines a "destory()" method. This method should be used instead of the @PostDestroy annotation.

        3. In this app, BeanProcessorDemoApp.java is the main program. TennisCoach.java is the prototype scoped bean. TennisCoach implements the DisposableBean interface and provides the destroy() method. The custom bean processing is handled in the MyCustomBeanProcessor class.

    - **The method should be no-arg**

---

# 3. Spring Configuration with No Xml

## Spring Configuration with Java Code (no xml)

### Setup Spring Container

- Create a Java class and annotate as @Configuration

    ```java
    @Configuration
    @ComponentScan("com.allen.springdemo") // optional
    public class PracSevenSpringConfig {
    	...
    }
    ```

- Add component scanning support: @ComponentScan (**Optional**)
    - automatically scan the package and find the beans - exactly same as xml @Component scanning (Scanning the package and register the class with @Component annotation)
- Read Spring Java configuration class in the main class.

    ```java
    // read spring config java class
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
    ```

- Retrieve bean from Spring container

## Defining Spring Beans with Java Code (no xml)

### Automatically Defining Beans - Using Component Scanning

- Using @Component and @Autowired
    - Similar steps in the above section.

### Manually Defining Beans - Without Using Component Scanning

- Define method to expose bean, use a configuration java class to replace the xml file.

    File: SpringConfig.java

    ```java
    @Configuration
    public class SpringConfig {

    	@Bean
    	public Coach swimCoach() {
    		SwimCoach mySwimCoach = new SwimCoach();
    		
    		return mySwimCoach;
    	}
    }
    ```

- Inject bean dependencies

    Coach will have fortune service dependency

    File: SpringConfig.java

    ```java
    @Configuration
    public class SpringConfig {	

    	// bean id happyFortuneService
    	@Bean
    	public FortuneService happyFortuneService() {
    		return new HappyFortuneService(); // return happyFortuneService;
    	}
    	
    	// bean id swimCoach
    	@Bean
    	public Coach swimCoach() { 
    		// inject happyFortuneService
    		SwimCoach mySwimCoach = new SwimCoach(happyFortuneService()); 
    		return mySwimCoach;
    	}
    }
    ```

    `@Bean` → telling Spring that we are creating a bean component manually

    `public Coach swimCoach() {}` → the bean id is defined by naming methodName. The return type is the Coach interface, this is useful for dependency injection

    `SwimCoach mySwimCoach = new SwimCoach();` → create a new instance of the SwimCoach
    `happyFortuneService()` → inject happyFortuneService 
    `return mySwimCoach;` → returns an instance of the swimCoach

    By default → Singleton. If the bean hasn't been created, all codes within the method (`swimCoach()`) will be executed. A flag (register the bean in the application context) will be set. When this method is called again, @Bean annotation will check in memory of the Spring container (applicationContext) and see if the given bean has already been created. Because the bean already has been created, it will not execute the code inside of the method.

    ```java
    return new SwimCoach(sadFortuneService()); 
    ```

    This code creates an instance of SwimCoach. Note the call to the method sadFortuneService(). We are calling the annotated method above. **The @Bean will intercept and return a singleton instance of sadFortuneService**. The sadFortuneService is then injected into the swim coach instance.

- 3. Read Spring Java configuration class

- 4. Retrieve bean from Spring container

## Injecting Values from properties File

- Create a properties file with values.
- Load Properties file in Spring config

    ```java
    @Configuration
    @PropertySource("classpath:sport.properties")
    public class SpringConfig {
    	...
    }
    ```

- Reference Values from properties File

    ```java
    public class SwimCoach implements Coach {
    	
    	private FortuneService fortuneService;
    	@Value("${foo.email}")
    	private String email;
    	@Value("${foo.team}")
    	private String team;
    	
    	...
    }
    ```