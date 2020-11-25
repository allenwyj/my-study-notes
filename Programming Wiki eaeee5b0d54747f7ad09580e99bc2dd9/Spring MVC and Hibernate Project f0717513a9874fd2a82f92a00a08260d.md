# Spring MVC and Hibernate Project

# Customer Relationship Management - CRM

## Customer Data Access Object

- Responsible for interfacing with the database
- This is a common design pattern: Data Access Object (DAO) - **Helper class / Utility class**

    Customer Controller ↔ Customer DAO ↔ Database

- We will use DAO as an API to access the database
- CRUD Methods
    - saveCustomer(...)
    - getCustomer(...)
    - getCustomers(...)
    - updateCustomer(...)
    - deleteCustomer(...)

## Hibernate Session Factory

Customer DAO ↔ Session Factory ↔ Data Source ↔ Database

- Hibernate Session Factory needs a Data Source
    - The data source defines database connection info
- Dependencies
    - There are all dependencies, we can wire them together with Dependency Injection (DI)
- Data Source
    - Tells how to connect to the database.

    ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled.png)

- Session Factory
    - inject the data source

    ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%201.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%201.png)

## Spring @Transactional

- Spring provides an @Transactional annotation
- Automagically begin and end a transaction for your Hibernate code
    - **no need to explicitly do the below job in our code**

        ```java
        // explicitly coding
        // start a transaction
        session.beginTransaction();

        // Hibernate stuff here

        // commit transaction
        session.getTransaction().commit();
        ```

- Spring will do this behind the scenes
- We still can manually control the transaction to handle our business logic without annotating @Transactional

## Specialised Annotation for DAOs

- Spring provides the @Repository annotation

    ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%202.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%202.png)

- Applied to DAO implementations
- Spring will automatically register the DAO implementation
    - because of component-sacnning
- Spring provides translation of any JDBC related exceptions

## New Annotations

### HTTP Request / Response

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%203.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%203.png)

- Sending data with GET method
    - Set up the form and the key item, and set the method GET

        ```html
        <form action="processForm" method="GET" ... >
        	...
        </form>
        ```

    - Form data is added to end of URL as name / value pairs

        `theUrl?field1=value1&field2=value2...`

### HTTP Request for POST data

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%204.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%204.png)

- Sending data with POST method
    - In this case, the form data is passed in the body of the HTTP request message
    - Set up the form, action equals processForm, and then method euqals POST

        ```html
        <form action="processForm" method="POST" ... >
        	...
        </form>
        ```

### Handling Form Submission

- In Spring, `@RequestMapping` mapping handles **ALL HTTP** methods

    ```java
    @RequestMapping("/processForm")
    public String processForm(...) {
    	...
    }
    ```

- Constrain the Request Mapping - **GET**
    - This mapping ONLY handles GET method
    - Any other HTTP REQUEST method will get rejected.

    ```java
    @RequestMapping(path="/processForm", method=RequestMethod.GET)
    public String processForm(...) {
    	...
    }
    ```

- Constrain the Request Mapping - **POST**
    - This mapping ONLY handles **POST** method
    - Any other HTTP REQUEST method will get rejected.

    ```java
    @RequestMapping(path="/processForm", method=RequestMethod.POST)
    public String processForm(...) {
    	...
    }
    ```

### @GetMapping - short-cut

- This mapping ONLY handles GET method
- Any other HTTP REQUEST method will get rejected.

```java
@GetMapping("/processForm")
public String processForm(...) {
	...
}
```

### @PostMapping

- This mapping ONLY handles POST method
- Any other HTTP REQUEST method will get rejected.

```java
@PostMapping("/processForm")
public String processForm(...) {
	...
}
```

### Pros and Cons of using GET or POST to send data

- GET
    - Good for debugging
        - you can see everything in the URL
    - Bookmark or email URL
    - Limitations on data length
        - in general, using about 1000 characters will be safe.
- POST
    - Can't booknmark or email URL
    - No limitations on data length
        - can have a very large form or very large piece of data to send
    - **Can also send binary data**
        - can use for sending file attachments or file uploads

## Refactor: add a Service Layer

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%205.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%205.png)

### Propose of Service Layer

- **Service Facade** design pattern
- Intermediate layer for custom business logic
- Integrate data from multiple sources (DAO / repositories)
    - Service layer can talk to different DAO objects to access different databases.
    - CustomerDAO, SalesDAO, LicenseDAO etc.

### @Service

- @Service applied to Service implementations
- Spring will automatically register the Service implementation due to the component-scanning.
- **How to build a Customer Service**
    - Define Service interface

        ```java
        public interface CustomerService {
        	
        	public List<Customer> getCUstomers();

        }
        ```

    - Define Service implementation
        - `@Autowired`Inject the CustomerDAO
        - `@Transactional` now moves to the service layer - Service is managing the transactions.
            - Good practice - use services and DAOs together, let the service defines, starts and ends the transactional layer, and then DAOs run the same context.
                - So we moves the define, start and end the transactions to the service layers, and then it'll make the appropriate calls inside the DAO class.
                - Delegate calls to DAO `return customerDAO.getCustomers();`
                - At the end, the service will clean up the transaction.

        ```java
        @Service
        public class CustomerServiceImpl implements CustomerService {

        	@Autowired
        	private CustomerDAO customerDAO;

        	@Transactional
        	public List<Customer> getCustomers() {
        		...
        		return customerDAO.getCustomers();
        	}
        }

        **// Updates for the DAO implementation**
        @Repository
        public class CustomerDAOImpl implements CustomerDAO {

        	@Autowired
        	private SessionFactory sessionFactory;
        	
        	// removed @Transactional
        	public List<Customer> getCustomers() {
        		...
        	}
        }
        ```

## Development Process

### Set up Database Dev Environment

### Create project and get all jar files ready.

- Choose back to the Java EE perspective
- New → Dynamic Web Project
- Adding JAR files
    - Copy Spring JAR files and paste to WebContent/WEB-INF/lib
    - Copy mysql-connector-java-xxxx.jar and paste in WebContent/WEB-INF/lib
    - Copy the two JSTL jar files to use HttpServletRequest , javax.servlet.jsp.jstl-1.2.1.jar, javax.servlet.jsp.jstl-api-1.2.1.jar.
    - Copy Hibernate validator JAR files

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%206.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%206.png)

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%207.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%207.png)

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%208.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%208.png)

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%209.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%209.png)

![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2010.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2010.png)

### Perform dbTest

1. create a new package and create a new servlet inside the package
    - uncheck the options

        ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2011.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2011.png)

    - `import java.sql.*;` - For all of jdbc classes
    - edit the codes and run on the server

        ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2012.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2012.png)

    - Error while starting the server - "May be locked by another process."
        - Highlight server and select 'add and remove', and remove all the projects.

### Configuration for Spring + Hibernate - i.e. spring-mvc-crud-demo-servlet.xml

1. Define database dataSource / connection pool
    - setup "myDataSource"
    - "driverClass"
    - "jdbcUrl"
    - "user" and "password"
    - setup the pool size
2. Setup Hibernate session factory
    - "sessionFactory"
    - define "dataSource", ref "myDataSource"
    - tell system where to scan for the @Entity classes, → "packagesToScan"
    - specify "hibernateProperties" - find all of our Hibernate entities
3. Setup Hibernate transaction manager
    - define "myTransactionManager", ref sessionFactory back to the "sessionFactory" defined in step 2.
4. Enable configuration of transactional annotations
    - @Transactional
        - allows you to minimise or eliminate some of your coding from manually starting and stopping transactions.
    - Simply reference our transaction manager and says our transaction manager is gonna be annotation-driven

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
    xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/mvc
		http://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/tx 
		http://www.springframework.org/schema/tx/spring-tx.xsd">

	<!-- MVC Step 3, 4, 5 -->
	<!-- 3.Add support for component scanning -->
	<context:component-scan base-package="com.luv2code.springdemo" />

	<!-- 4.Add support for conversion, formatting and validation support -->
	<mvc:annotation-driven/>

	<!-- 5.Define Spring MVC view resolver -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view/" />
		<property name="suffix" value=".jsp" />
	</bean>

    <!-- Step 1: Define Database DataSource / connection pool -->
	<bean id="myDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
          destroy-method="close">
        <property name="driverClass" value="com.mysql.cj.jdbc.Driver" />
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/web_customer_tracker?useSSL=false&amp;serverTimezone=UTC" />
        <property name="user" value="springstudent" />
        <property name="password" value="springstudent" /> 

        <!-- these are connection pool properties for C3P0 -->
        <property name="minPoolSize" value="5" />
        <property name="maxPoolSize" value="20" />
        <property name="maxIdleTime" value="30000" />
	</bean>  
	
    <!-- Step 2: Setup Hibernate session factory -->
	<bean id="sessionFactory"
		class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
		<property name="dataSource" ref="myDataSource" />
		<property name="packagesToScan" value="com.luv2code.springdemo.entity" />
		<property name="hibernateProperties">
		   <props>
		      <prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
		      <prop key="hibernate.show_sql">true</prop>
		   </props>
		</property>
   </bean>	  

    <!-- Step 3: Setup Hibernate transaction manager -->
	<bean id="myTransactionManager"
            class="org.springframework.orm.hibernate5.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory"/>
    </bean>
    
    <!-- Step 4: Enable configuration of transactional behavior based on annotations -->
	<tx:annotation-driven transaction-manager="myTransactionManager" />

</beans>
```

### Test Spring MVC Controller Before Connecting with the Hibernate

- Because sometimes Tomcat will have catching problem, so we need to test it.
- Create a controller and view/list-customer.jsp

    ```java
    @Controller
    @RequestMapping("/customer")
    public class CustomerController {

    	@RequestMapping("/list")
    	public String listCustomers(Model theModel) {
    		return "list-customer"; // returning the name of the jsp file.
    	}
    }
    ```

### List Customers

1. Create Customer.java
    - Mapping the class and fields to the database
        - id must be `@GeneratedValue(strategy = GenerationType.**IDENTITY**)`
    - Configure entity scanning in the config file.
2. Create CustomerDAO.java and CustomerDAOImpl.java
    - Define DAO interface

        ```java
        public interface CustomerDAO {

        	public List<Customer> getCustomers();
        }
        ```

    - Define DAO implementation
        - `@Autowired` for Dependency Injection
            - We inject the session factory into this actual DAO. So Spring will actually look into the configuration, and see if there is a bean id named sessionFactory that will inject that bean into this given DAO implementation.
        - **NOTE!!!** check the refactor section to implement a service layer.

        ```java
        package com.allen.springdemo.dao;

        import java.util.List;

        import org.hibernate.Session;
        import org.hibernate.SessionFactory;
        import org.hibernate.query.Query;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Repository;
        //import org.springframework.transaction.annotation.Transactional;

        import com.allen.springdemo.entity.Customer;

        @Repository
        public class CustomerDAOImpl implements CustomerDAO {

        	// need to inject the session factory, name should match the bean id
        	@Autowired
        	private SessionFactory sessionFactory;
        	
        	@Override
        	// @Transactional // we defined this in the service layer.
        	public List<Customer> getCustomers() {

        		// get the current hibernate session
        		Session currentSession = sessionFactory.getCurrentSession();
        		
        		// create a query
        		Query<Customer> theQuery = currentSession.createQuery("from Customer", Customer.class);
        		
        		// execute the query
        		List<Customer> customers = theQuery.getResultList();
        		
        		// return the results
        		return customers;
        	}

        }
        ```

3. Create CustomerService.java and CustomerServiceImpl.java - Check the service refactor
    - Define Service interface

        ```java
        public interface CustomerService {
        	
        	public List<Customer> getCUstomers();

        }
        ```

    - Define Service implementation
        - service layer will handle define, start and end of transactions. DAO will still be used to fetch data from the database.

        ```java
        @Service
        public class CustomerServiceImpl implements CustomerService {

        	// need to inject customerDAO
        	@Autowired
        	private CustomerDAO customerDAO;
        	
        	@Override
        	@Transactional
        	public List<Customer> getCustomers() {
        		
        		// delegate to the dao
        		return customerDAO.getCustomers();
        	}

        }
        ```

4. Create CustomerController.java
    - Inject Customer Service into the controller
        - @Autowired, Spring will scan for a component that implements CustomerService interface
        - `theModel.addAttribute("customers", theCustomer)`, customers should match the looping items' name in the jsp file.

        ```java
        @Controller
        @RequestMapping("/customer")
        public class CustomerController {
        	
        /**
        	// need to inject the customer dao
        	@Autowired
        	private CustomerDAO customerDAO; **/
        	
        	// need to inject the customer service now
        	@Autowired
        	private CustomerService customerService;
        	
        	@RequestMapping("/list")
        	public String listCustomers(Model theModel) {
        		
        		// get customers from the dao
        		List<Customer> theCustomers = customerService.getCustomers();
        		
        		// add the customers to the model
        		theModel.addAttribute("customers", theCustomers);
        		
        		return "list-customer";
        	}
        	
        }
        ```

5. Create JSP page: list-customers.jsp

    ```html
    <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

    <!DOCTYPE html>
    <html>
    <head>
    	<title>List Customers</title>
    </head>

    <body>
    	<div id="wrapper">
    		<div id="header">
    			<h2> CRM - Customer Relationship Manager </h2>
    			
    			<!--  add out html table  -->
    			<table>
    				<tr>
    					<th>First Name</th>
    					<th>Last Name</th>
    					<th>Email</th>
    				</tr>
    			
    				<!-- loop over and print our customers -->
    				<c:forEach var="tempCustomer" items="${customers}">
    					<tr>
    						<td>${tempCustomer.firstName}</td>
    						<td>${tempCustomer.lastName}</td>
    						<td>${tempCustomer.email}</td>
    					</tr>
    				</c:forEach>
    			</table>
    		</div>
    	</div>
    </body>
    </html>
    ```

6. Apply CSS in the web app
    1. Place CSS in a 'resources' directory

        ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2013.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2013.png)

    2. Configure Spring to serve up 'resources' directory
        - location gives the phtsical directory name
        - mapping gives URL mappings that will come in throught the browser.
            - ** means it's going to actually recurse and apply this to all subdirectories.

        ```xml
        File: spring-mvc-demo-servlet.xml
        <mvc:resources location="/resources/" mapping="/resources/**" />
        ```

    3. Reference CSS in your JSP
        - normally reference a css file in the html file.
        - `${pageContext.request.contextPath}` - JSP expression gives the correct name of our application.

        ```html
        <link type="text/css"
        		rel="stylesheet"
        		href="${pageContext.request.contextPath}/resources/css/style.css">
        ```

    - Special Note (NOT REQUIRED):

        ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2014.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2014.png)

        ![Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2015.png](Spring%20MVC%20and%20Hibernate%20Project%20f0717513a9874fd2a82f92a00a08260d/Untitled%2015.png)

---

### Add Customer

1. Update list-customer.jsp
    - New "Add Customer" button

        ```html
        <!-- put new button: Add Customer -->
        <input type="button" value="Add Customer" 
        		onclick="window.location.href='showFormForAdd'; return false;"
        		class="add-button"
        />
        ```

2. Direct to showFormForAdd page in the controller
    - we will pass the model to the form to bind the data.

    ```java
    	@GetMapping("/showFormForAdd")
    	public String showFormForAdd(Model theModel) {
    		
    		// create model attribute to bind form data
    		Customer theCustomer = new Customer();
    		
    		theModel.addAttribute("customer", theCustomer);
    		
    		return "customer-form";
    	}
    ```

3. Create HTML form for new customer
    - `action="saveCustomer"` - Send to Spring MVC mapping
    - `modelAttribute="customer"` - Bind the data to this modelAttribute item in the controller

    ```html
    <%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>

    <!DOCTYPE html>
    <html>
    <head>
    	<title>Save Customer</title>
    	
    	<link type="text/css"
    			rel="stylesheet"
    			href="${pageContext.request.contextPath}/resources/css/style.css" >
    			
    	<link type="text/css"
    			rel="stylesheet"
    			href="${pageContext.request.contextPath}/resources/css/add-customer-style.css" >
    </head>

    <body>
    	<div id="wrapper">
    		<div id="header">
    			<h2>CRM - Customer Relationship Manager</h2>
    		</div>
    	</div>
    	<div id="container">
    		<h3>Save Customer</h3>
    		<form:form action="saveCustomer" modelAttribute="customer" method="POST"> 
    		
    			<table>
    				<tr>
    					<td><label>First Name:</label></td>
    					<td><form:input path="firstName" /></td>
    				</tr>
    				
    				<tr>
    					<td><label>Last Name:</label></td>
    					<td><form:input path="lastName" /></td>
    				</tr>
    				
    				<tr>
    					<td><label>Email:</label></td>
    					<td><form:input path="email" /></td>
    				</tr>
    				
    				<tr>
    					<td><label></label></td>
    					<td><input type="submit" value="Save" class="save" /></td>
    				</tr>
    			</table>
    		</form:form>

    		<div style="clear; both;"></div>
    		<p>
    			<a href="${pageContext.request.contextPath}/customer/list">Back to List</a>
    		</p>	
    	</div>
    </body>
    </html>
    ```

4. Process Form Data
    - Controller → Service → DAO
    - CustomerController.java
        - the form action `saveCustomer` will be mapped to this method
        - we save the customer using our service, customerService will auto-find the class which implements the CustomerService interface.

        ```java
        @PostMapping("/saveCustomer")
        public String saveCustomer(@ModelAttribute("customer") Customer theCustomer) {
        	
        	// save the customer using our service
        	customerService.saveCustomer(theCustomer);
        	
        	return "redirect:/customer/list";
        }
        ```

    - Update CustomerServiceImpl.java and CustomerDAOImpl.java
        - we open the transaction in the service and delegate to the DAOimpl
        - DAOImpl save the customer into the database.

        ```java
        // we open the transaction in the service and delegate to the DAOimpl
        @Override
        @Transactional
        public void saveCustomer(Customer theCustomer) {
        	
        	customerDAO.saveCustomer(theCustomer);
        }
         ---------------------------

        // DAOImpl save the customer into the database.
        @Override
        public void saveCustomer(Customer theCustomer) {
        	
        	// get current session
        	Session currentSession = sessionFactory.getCurrentSession();
        	
        	// save the customer
        	currentSession.save(theCustomer);
        }
        ```

    - Update two interfaces.

### Sort Customer

- sort customer by last name - we can do this in the DAOimpl class by modifying the query

    ```java
    @Override
    public List<Customer> getCustomers() {

    	// get the current hibernate session
    	Session currentSession = sessionFactory.getCurrentSession();

    	// create a query sort by last name
    	Query<Customer> theQuery = currentSession.createQuery("from Customer **order by lastName**", Customer.class);
    	
    	// execute the query
    	List<Customer> customers = theQuery.getResultList();
    	
    	// return the results
    	return customers;
    }
    ```

### Update Customer

1. Update list-customer.jsp
    - New "Update" link
        - `<c:url >...</c:url>` : create a url and assign it to a variable so we can use it later
            - `var="updateLink"` : variable name, so we can use it later
            - `value="/customer/showFormForUpdate"` : the actual value is the mapping or url that we are building here.
            - `<c:param name="customerId" value="${tempCustomer.id}" />` : create a url's parameter called customerID, assign its value by looping tempCustomer and getting tempCustomer.id. Then, pass it to the updateLink which is mapping to /customer/showFormForUpdate.
    - Display the update link in the table

    ```html
    <!-- loop over and print our customers -->
    <c:forEach var="tempCustomer" items="${customers}">
    	<!-- construct an "update" link with customer id -->
    	<c:url var="updateLink" value="/customer/showForForUpdate" >
    		<c:param name="customerId" value="${tempCustomer.id}" />
    	</c:url>
    	
    	<tr>
    		<td>${tempCustomer.firstName}</td>
    		<td>${tempCustomer.lastName}</td>
    		<td>${tempCustomer.email}</td>
    		<td>
    			<!-- display the update link -->
    			<a href="${updateLink}">Update</a>
    		</td>
    	</tr>
    </c:forEach>
    ```

2. Retrieve customer from the database and prepopulate to the form
    - taking url mapping and handle the parameter "customerId" in the CustomerController.java

        ```java
        @GetMapping("/showFormForUpdate")
        public String showFormForUpdate(@RequestParam("customerId") int theId, Model theModel) {
        	
        	// get the customer from the service
        	Customer theCustomer = customerService.getCustomer(theId);
        	
        	// set customer as a model attribute to pre-populate the form
        	theModel.addAttribute("**customer**", theCustomer);
        	
        	// send over to our form
        	return "customer-form";
        }
        ```

    - implementation getCustomer(int id) in the CustomerServiceImpl and CustomerDAOImpl

        ```java
        File: CustomerServiceImpl.java
        @Override
        @Transactional
        public Customer getCustomer(int theId) {
        	
        	return customerDAO.getCustomer(theId);
        	
        }

        File: CustomerDAOImpl.java
        @Override
        public Customer getCustomer(int theId) {
        	
        	// get current session
        	Session currentSession = sessionFactory.getCurrentSession();
        	
        	// get the customer by id
        	Customer theCustomer = currentSession.get(Customer.class, theId);
        	
        	return theCustomer;
        }
        ```

    - setup the interfaces
3. Update customer-form.jsp
    - Prepopulate the form : customer-form.jsp
        - When the form is **loaded**
            - Spring MVC will see the model attribute "customer" is not null. so it will call customer.getFirstName() ... etc.
            - Then, put the value on the form
            - If the model value is null, it won't show any value.
        - When the form is **submitted**
            - Spring MVC will call customer.setFirstName() ... etc.
4. Process form data for updating
    - Controller → Service → DAO
    - It's important to get the customer id when we pre-populate the form, because we might lose the context or lose the original customer if we don't track the customer id. By tracking this id, the system could know which customer is to form the update operation.
        - therefore, we setup a hidden form to save the id.

        ```html
        <form:form action="saveCustomer" modelAttribute="customer" method="POST">
        	<!-- need to associate this data with customer id -->
        	**<form:hidden path="id" />**
        	...
        </form:form>
        ```

    - Update the CustomerDAOImpl.java
        - `session.save(...)` - only handle create a new record in the database (Insert)
        - `session.update(...)`- only handle update a existing record in the database (Update)
        - SO, we need to update our code and use `saveOrUpdate(...)` - if(primaryKey / id) empty, then INSERT new record, else UPDATE esisting record

        ```java
        @Override
        public void saveCustomer(Customer theCustomer) {
        	
        	// get current session
        	Session currentSession = sessionFactory.getCurrentSession();
        	
        	// saveOrUpdate the customer
        	currentSession.saveOrUpdate(theCustomer);
        }
        ```

### Delete Customer

1. Add "delete" link on JSP
    - Add JavaScript to prompty the user
        - confirm(...) : displays a confirmation popup dialog
            - if user hit the cancel, it will return false
            - if user hit the ok, it will keep proceeding the codes

            ```html
            <!-- construct an "delete" link with customer id -->
            <c:url var="deleteLink" value="/customer/delete" >
            	<c:param name="customerId" value="${tempCustomer.id}" />
            </c:url>

            <a href="${deleteLink}"
            	onclick="if (!(confirm('Are you sure you want to delete this customer?'))) return false">Delete</a>
            ```

2. Add code for "delete"
    - Controller → Service → DAO
    - similar steps for updating 4 classes.
        - `theQuery.executeUpdate();` - For general purpose
        - `:customerId` should match `theQuery.setParameter("customerId", theId);`

        ```java
        File: CustomerDAOImpl.java

        @Override
        public void deleteCustomer(int theId) {
        	
        	// get the current session
        	Session currentSession = sessionFactory.getCurrentSession();
        	
        	// delete customer by id
        	Query theQuery = currentSession.createQuery("delete from Customer where id=:customerId");
        	theQuery.setParameter("customerId", theId);
        	
        	theQuery.executeUpdate();
        }

        File: CustomerController.java

        @GetMapping("/delete")
        public String deleteCustomer(@RequestParam("customerId") int theId) {
        	
        	// delete the customer
        	customerService.deleteCustomer(theId);
        	
        	return "redirect:/customer/list";
        }
        ```

### Search Customer

1. Create the HTML form - list-customers.jsp
    - add a search form to read user input and submit it to Spring controller mapping

        ```html
        <%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>

        <!--  add a search box -->
        <form:form action="search" method="GET">
            Search customer: <input type="text" name="theSearchName" />
            
            <input type="submit" value="Search" class="add-button" />
        </form:form>
        ```

2. Add mapping to the controller

    ```java
    File: CustomerController.java
    @GetMapping("/search")
    public String searchCustomers(@RequestParam("theSearchName") String theSearchName, Model theModel) {

        // search customers from the service
        List<Customer> theCustomers = customerService.searchCustomers(theSearchName);

        // add the customers to the model
        theModel.addAttribute("customers", theCustomers);

        return "list-customers";        
    }
    ```

3. Add methods in the service layer to delegate to DAO
    - update interface methods
    - add method to impl classes

    ```java
    File: CustomerServiceImpl.java
    @Override
    @Transactional
    public List<Customer> searchCustomers(String theSearchName) {
    	
    	return customerDAO.searchCustomers(theSearchName);
    }

    File: CustomerDAOImpl.java
    @Override
    public List<Customer> searchCustomers(String theSearchName) {
    	
    	// get the current hibernate session
      Session currentSession = sessionFactory.getCurrentSession();

      Query theQuery = null;

      //
      // only search by name if theSearchName is not empty
      //
      if (theSearchName != null && theSearchName.trim().length() > 0) {
    		
      	// search for firstName or lastName ... case insensitive
        theQuery = currentSession.createQuery("from Customer where lower(firstName) like :theName or lower(lastName) like :theName", Customer.class);
        theQuery.setParameter("theName", "%" + theSearchName.toLowerCase() + "%");

      }
      else {
        // theSearchName is empty ... so just get all customers
        theQuery =currentSession.createQuery("from Customer", Customer.class);            
      }

      // execute query and get result list
      List<Customer> customers = theQuery.getResultList();

      // return the results        
      return customers;
          
    }
    ```