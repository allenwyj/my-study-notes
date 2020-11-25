# Aspect-Oriented Programming- AOP

# What is AOP

- Programming technique based on concept of an Aspect
- Aspect encapsulates cross-cutting logic
- **Cross-Cutting Concerns**
    - "Concern" means logic / functionality
- Spring框架的AOP机制可以让开发者把业务流程中的通用功能抽取出来，单独编写功能代码。在业务流程执行过程中，Spring框架会根据业务流程要求，自动把独立编写的功能代码切入到流程的合适位置。例如用户登录功能。

## Aspects

- Aspect can be reused at multiple locations
- Same aspect / class ... applied based on configuration

## AOP Solution

- Apply the Proxy design pattern

    ![Aspect-Oriented%20Programming-%20AOP%20cbd7e6f1dcdc418890f56f69f6798de0/Untitled.png](Aspect-Oriented%20Programming-%20AOP%20cbd7e6f1dcdc418890f56f69f6798de0/Untitled.png)

## Benefits of AOP

- **Code for Aspect is defined in a single class**
    - Much better than being scattered everywhere
    - Promotes code reuse and easier to change
- **Business code in your application is cleaner**
    - Only applies to business functionality: addAccount
    - Reduces code complexity
- **Configurable**
    - Based on configuration, apply Aspects selectively to different parts of app
    - No need to make changes to main application code ... very important.

## Additional AOP Use Cases

- **Most common**
    - logging, security, tanrasactions
- **Audit logging**
    - who, what, when, where
- **Exception handling**
    - log exception and notify DevOps team via SMS / email
- **API Management**
    - how many times has a method been called user
    - analytics: what are peak times? what is average load? who is top user?

## AOP: Advantages and Disadvantages

### Advantages

- Reusable modules
- Resolve code tangling
- Resolve code scatter
- Applied selectively based on configuration

### Disadvantages

- Too many aspects and app flow is hard to follow
    - many developer are working on the same project and create many aspects for themselves. (solution: use guideline,  governance to manage the aspects)
- Minor performance cost for aspect execution (run-time weaving)
    - if we have small number of aspects, it won't be a problem. If we have a large number of apsects, we will feel the time cost.

## AOP Terminology

- **Aspect**: module of code for a cross-cutting concern (logging, security, ... )
- **Advice**: what action is taken and when it should be applied
- **Join Point**: when to apply code during program execution
- **Pointcut**: A predicate expression for where advice should be applied.

## Advice Types

- **Before advice :** run before the actual method executes
- **After finally advice:** run after the method finishes (like the finally block in the try-catch)
- **After returning advice:** run after the method for a success execution)
- **After throwing advice :** run after method finished, if an exception is thrown
- **Around advice :** run before and after method

## Weaving

- Connecting aspects to target objects to create an advised object
- Different types of weaving
    - Compile-time, load-time or run-time
- Regarding performance: run-time weaving is the slowest.

## Comparing Spring AOP and AspectJ

### AOP Frameworks

- Two leading AOP Frameworks for Java (Spring AOP and AspectJ)

### Spring AOP Support

- Spring provides AOP support
- Key component of Spring
    - Security, transactions, caching etc
- Uses run-time weaving of aspects
    - Spring makes use of the Proxy pattern to advice an object

    ![Aspect-Oriented%20Programming-%20AOP%20cbd7e6f1dcdc418890f56f69f6798de0/Untitled%201.png](Aspect-Oriented%20Programming-%20AOP%20cbd7e6f1dcdc418890f56f69f6798de0/Untitled%201.png)

### AspectJ

- Provides complete support for AOP
- Rich support for
    - join points: method-level, constructor, field
    - code weaving: compile-time, post compile-time and load-time

### Spring AOP Comparison

**Advantages**

- Simpler to use than AspectJ
- Uses Proxy pattern
- Can migrate to AspectJ when using @Aspect annotation

**Disadvantages**

- Only supports method-level join points
- Can only apply aspects to beans created by Spring app context
- Minor performance cost for aspect execution (run-time weaving)
    - because Spring AOP makes use of run-time weaving.)

### AspectJ Comparison

- AspectJ is very fast but a lot of complexity with it.

**Advantages**

- Support all join points
- Works with any POJO not just beans from app context
- Faster performance compared to Spring AOP
- Complete AOP support
    - the full stack, the full API

**Disadvantages**

- Compile-time weaving requires extra compilation step
- AspectJ pointcut syntax can become complex

### Comparing Spring AOP and AspectJ

- Spring AOP only supports
    - Method-level join points
    - Run-time code weaving (slower thatn AspectJ)
- AspectJ supports
    - join points: method-level, constructor, field
    - weaving: compile-time, post compile-time and load-time.
- Spring AOP is a light implementation of AOP
- Solves common problems in enterprise applications
- Recommendation:
    - Start with Spring AOP, easy to use
    - if having complex requirements then move to AspectJ

---

# Advice Types

## Add JAR files

- even though we are using Spring AOP, we still need to add AspectJ JAR files, because Spring AOP is the light implementation of AspectJ.
- [https://mvnrepository.com/artifact/org.aspectj/aspectjweaver](https://mvnrepository.com/artifact/org.aspectj/aspectjweaver)

## @Before Advice - Interaction

### Use Cases

- Most common
    - Logging, security, transactions
- Audit logging
    - who, what, when, where
- API Management
    - how many times has a method been called user
    - analtical

### Development Process - @Before

1. Create Target Object: AccountDAO under com.allen.aopdemo.dao

    ```java
    @Component
    public class AccountDAO {
    	public void addAccount() {
    		System.out.println("Doing my db work: adding an account");
    	}
    }
    ```

2. Create Spring Java Config class
    - Using pure java code configuration under com.allen.aopdemo

    ```java
    @Configuration
    @EnableAspectJAutoProxy
    @ComponentScan("com.allen.aopdemo")
    public class DemoConfig {

    }
    ```

3. Create main app

    ```java
    public class MainDemoApp {

    	public static void main(String[] args) {
    		// read spring config java class
    		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DemoConfig.class);
    		
    		// get the bean from spring container
    		AccountDAO  theDAO = context.getBean("accountDAO", AccountDAO.class);
    		// call the business method
    		theDAO.addAccount();
    		
    		// close the context
    		context.close();
    	}
    }
    ```

4. Create an Aspect with @Before advice
    - run this code BEFORE - target object method: "public void addAccount()"

    ```java
    @Aspect
    @Component
    public class MyDemoLoggingAspect {
    	@Before("execution(public void addAccount())") // Pointcut expression
    	public void beforeAddAccountAdvice() {
    		// own business logic
    	}
    }
    ```