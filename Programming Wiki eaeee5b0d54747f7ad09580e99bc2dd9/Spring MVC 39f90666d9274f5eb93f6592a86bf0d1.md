# Spring MVC

# Spring MVC

## The MVC Graph

![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled.png)

## Spring MVC Benefits

- Building in java
- Leverage a set of reusable UI components
- Help manage application state for web requests
- process form data: validation, conversion etc
- Flexible configuration for the view layer

## Front Controller

- Known as DispatcherServlet
    - Part of the Spring Framework
    - Already developed by Spring Dev Team
- Need to creat:
    - Model objects
    - View templates
    - Controller classes

## Controller

- Contains your business logic
    - Handle the request
    - Store / retrieve data (db, web service...)
    - Place data in model
- Send to appropriate view template

## Model

- Contains data
- Store / retrieve data via backend systems
    - database, web service, etc..
    - Use a Spring bean if you like
- Place your data in the model
    - Data can be any Java object/ collection

## View template

- Spring MVC is flexible
    - many view templates (Thymeleaf, Groovy, Vleocity, Freemarker, etc...)
- Most common is **JSP** + **JSTL**
    - JSP: Java Server Pages
    - JSTL: JSP Standard Tag Library
- Developer creates a page
    - Displays data

    ---

# Spring MVC Configuration - Build an App

## Setup web.xml & servlet.xml

[Spring MVC Configuration](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b.md)

- Copy over Spring JAR files and paste into WEB-INF/lib

### web.xml

- Add configurations to file: WEB-INF/web.xml
    1. configure Spring MVC Dispatcher Servlet
        - add an entry for the Spring DispatcherServlet, or the Front Controller
        - place a name and class
        - set up initial parameter → tell it where the Spring context configuration file is located. any name is acceptable.
    2. set up URL mappings to Spring MVC Dispatcher Servlet
        - tell the system about if there is any URL pattern coming in, pass it off to the DispatcherServlet.
        - "/" → all requests coming in should be handled by the DispatcherServlet.
        - servlet-name should match the name in Step 1.

    File: web.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
    	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
    	id="WebApp_ID" version="3.1">

    	<display-name>spring-mvc-demo</display-name>

    	<absolute-ordering />

    	<!-- Spring MVC Configs -->

    	<!-- Step 1: Configure Spring MVC Dispatcher Servlet -->
    	<servlet>
    		<servlet-name>dispatcher</servlet-name>
    		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    		<init-param>
    			<param-name>contextConfigLocation</param-name>
    			<param-value>/WEB-INF/spring-mvc-demo-servlet.xml</param-value>
    		</init-param>
    		<load-on-startup>1</load-on-startup>
    	</servlet>

    	<!-- Step 2: Set up URL mapping for Spring MVC Dispatcher Servlet -->
    	<servlet-mapping>
    		<servlet-name>dispatcher</servlet-name>
    		<url-pattern>/</url-pattern>
    	</servlet-mapping>
    	
    </web-app>
    ```

### spring-mvc-demo-servlet.xml

- Add configurations to file: WEB-INF/spring-mvc-demo-servlet.xml

      3. Add support for Spring component scanning

    - scan for this package, for any special Spring beans, and make them available. → Looking for @Component, and put them into the Spring context

      4. Add support for conversion, formatting and validation

      5. Configure Spring MVC View Resolver

    - tell where the pages located.
    - tell Spring MVC to prepend the prefix and append the suffix

    File: spring-mvc-demo-servlet.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    	xmlns:context="http://www.springframework.org/schema/context"
    	xmlns:mvc="http://www.springframework.org/schema/mvc"
    	xsi:schemaLocation="
    		http://www.springframework.org/schema/beans
        	http://www.springframework.org/schema/beans/spring-beans.xsd
        	http://www.springframework.org/schema/context
        	http://www.springframework.org/schema/context/spring-context.xsd
        	http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    	<!-- Step 3: Add support for component scanning -->
    	<context:component-scan base-package="com.allen.springdemo" />

    	<!-- Step 4: Add support for conversion, formatting and validation support -->
    	<mvc:annotation-driven/>

    	<!-- Step 5: Define Spring MVC view resolver -->
    	<bean
    		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    		<property name="prefix" value="/WEB-INF/view/" />
    		<property name="suffix" value=".jsp" />
    	</bean>
    	
    	<!-- Load custom message resources -->
    	<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    	
    		<property name="basenames" value="resources/messages" />
    		
    	</bean>

    </beans>
    ```

## Creating Controllers and Views.

### @Controller

- Step 1 : Create Controller class
    - Annotate class with @Controller
        - `@Controller` inherits from `@Component`, supports scanning

        ```java
        @Controller
        public class HomeController {
        	...
        }
        ```

- Step 2 : Define Controller method

    ```java
    @Controller
    public class HomeController {

    	public String anyMethodName() {
    		...
    	}
    }
    ```

### @RequestMapping

- Step 3 : Add Request Mapping to Controller method

    Annotation maps a path to a method name 

    ```java
    @Controller
    public class HomeController {

    	@RequestMapping("/")
    	public String anyMethodName() {
    		...
    		return "main-menu";
    	}
    }
    ```

    - If we set a `@RequestMapping(...)` in the class level, the url will be "xxx/classMapping/methodMapping"
        - http://localhost:8080/spring-mvc-demo/ : will call mainPage()
        - http://localhost:8080/spring-mvc-demo/about : will call aboutPage()

        ```java
        @Controller
        @RequestMapping("/home")
        public class HomeController {

        	@RequestMapping("/")
        	public String mainPage() {
        		...
        		return "main-menu";
        	}

        	@RequestMapping("/about")
        	public String aboutPage() {
        		...
        		return "about-page";
        	}
        }
        ```

- Step 4 : Return View Name
    - `return "viewPageName"`;.
- Step 5 : Develop View Page
    - inside view folder, create viewFile.jsp which file name matches the return name.

## Reading HTML Form Data

- Create Controller class
- Show HTML form
    - Create controller method to show HTML Form
    - Create View Page for HTML form

### Reading HTML form field

- Process HTML Form
    - Create controller method to process HTML Form
        - You can read the name of HTML form field by using `${param.filedName}` in view page.
            - the `param` is the model attribute which is added in the controller class and pass to the view page.
    - Develop View Page for Confirmation

### Reading HTML form field - Using HttpServletRequest

- Passing data between controller and view
    - Using `HttpServletRequest request`
    - Using `@RequestParam("studentName") String theName` **- Preferred**
        - studentName 直接就pass给theName，直接拿来用
        - `model.addAttribute("message", result);` 将message为key ，result为value都传给接下来要return的view page

    ```html
    <form action="processFormVersionThree" method="GET">
    	
    		<input type="text" name="studentName"
    			placeholder="What's your name?" />
    		
    		<input type="submit" />
    		
    </form>
    ```

    ```java
    // new a controller method to read form data and add data to the model
    @RequestMapping("/processFormVersionTwo")
    public String letsShoutDude(HttpServletRequest request, Model model) {
    	
    	// read the request parameter from the HTML form
    	String theName = request.getParameter("studentName");
    	
    	// convert the data to all caps
    	theName = theName.toUpperCase();
    	
    	// create the message 
    	String result = "Yo! " + theName;
    	
    	// add message to the model
    	model.addAttribute("message", result);
    	
    	return "helloworld";
    }

    // new a controller method to read form data and add data to the model
    @RequestMapping("/processFormVersionThree")
    public String processFormVersionThree(
    		@RequestParam("studentName") String theName, Model model) {

    	// convert the data to all caps
    	theName = theName.toUpperCase();
    	
    	// create the message 
    	String result = "Welcome! " + theName;
    	
    	// add message to the model
    	model.addAttribute("message", result);
    	
    	return "helloworld";
    }
    ```

    ```html
    **File: helloworld.jsp**
    <!DOCTYPE html>
    <html>
    <body>
    Hello World of Spring!
    <br><br>
    Student name: ${param.studentName}
    <br><br>
    The message: ${message}
    </body>
    </html>
    ```

### Reading HTML Form Data with @RequestParam Annotation

- Bind variable using @RequestParam Annotation
    - Spring will read param from request: studentName
    - Bind it to the variable: theName

    ```java
    // Assign theName to value of request parameter
    @RequestMapping("/processFormVersionThree")
    	public String processFormVersionThree(
    			@RequestParam("studentName") String theName, Model model) {

    		...

    	}
    ```

## Adding Data to the Spring Model

### Passing Model to your Controller

- controller params are flexible, you can add any param if you want
    - **`HttpServletRequest request` → Holds HTML form data**
        - `${param.valuePassedFromHtmlForm}` - access from the view page.
    - `Model model` → Container for **your data**
    - `request.getParameter("fieldName")`
        - In controller, read the request parameter from the HTML form if you are using HttpServletRequest.
    - `model.addAttribute("Any Attribute Name", "value");`
        - can add many data as you want
        - can add any type of data if you want
        - the attribute name will be use to read the data in the jsp file.
    - Inside the view page, the attribute in the model can be accessed by using: `${**Any Attribute Name**}` - access data from the model

    File: xXXController.java

    ```java
    @Controller
    public class HelloWorld() {
    	// new a controller method to read form data and add data to the model
    	@RequestMapping("/processFormVersionTwo")
    	public String letsShoutDude(HttpServletRequest request, Model model) {
    		
    		// read the request parameter from the HTML form
    		String theName = request.getParameter("studentName");
    		
    		// convert the data to all caps
    		theName = theName.toUpperCase();
    		
    		// create the message 
    		String result = "Yo! " + theName;
    		
    		// add message to the model
    		model.addAttribute("message", result);
    		
    		return "helloworld";
    	}
    }
    ```

    [FAQ: How to use CSS, JavaScript and Images in Spring MVC Web App](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/FAQ%20How%20to%20use%20CSS,%20JavaScript%20and%20Images%20in%20Sprin%2058b824c1b0484a01ab9bf7d8222b2213.md)

    [**Deploying your App to Tomcat as a Web Application Archive (WAR) file**](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Deploying%20your%20App%20to%20Tomcat%20as%20a%20Web%20Application%20%20c075d91bbc094ee5a5e7ecdb9f749a7e.md)

## Adding Request Mappings to Controller

- Serves as parent mapping for controller
- All request mappings on methods in the controller are relative
- similar to folder directory structures

![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%201.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%201.png)

- need to change the path in the view(jsp file) to match the request mapping
- The HTML form uses "processForm" for the form action. **Notice that it does not have a forward slash, as a result, this will be relative to the current browser URL**

## Spring MVC Form Tags

- Spring MVC Form Tags are the building block for a web page
- Form Tags are configurable and reusable for a web page
- Can make use of data binding
    - Automatically setting
    - Retrieving data from a Java object or beans
- form tag will generat html codes for us.
- web page structure
    - inside html block.
- reference Spring MVC Form Tags - specify the Spring namespace at beginning of JSP fiel
    - `<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>`

### Showing Form

- In Spring Controller,
    - before you show the form, you must add a **model attribute**
        - This is a bean that will hold form data for the **data binding**
- Data Binding: modelAttribute in jsp file needs to match the showForm name, and the processForm
- path="firstName" → binding these form field to the property of bean

![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%202.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%202.png)

### Different types of form tag

- form:form - main form container
- form:inpu - text field
- form:textarea - multi-line text field
- form:checkbox - check box
- form:radiobutton - radio buttons
- form:select - drop down list
- more...

### form:input

![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%203.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%203.png)

- when the form is loaded, path="firstName" → student.getFirstName() get called.
- if the value of field is empty, it will show nothing.

- **when the form submitted, Spring MVC will call setter methods.**
- when you have form fields with duplicate path/name, then you will get a comma-delimited list of the data. The data all maps back to the same path/name. i.e. Brazil,BR
- @ModelAttribute: bind form data to object

![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%204.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%204.png)

### form:select

- Spring mvc will call the getter and setter for the fields.
- Spring will call student.getCountryOptions to get the value of countryOptions.

File: student-form.jsp

```html
Country: 
	<form:select path="country" >
	
		<form:option value="Brazil" label="Brazil" />
		<form:option value="India" label="India" />
		<form:option value="Germany" label="Germany" />
		<form:option value="France" label="France" />
		
		<!-- Using list to store country list--> 
		<form:options items="${student.countryOptions}" label="Brazil" />
	</form:select>
```

[How to use properties file to load country options](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/How%20to%20use%20properties%20file%20to%20load%20country%20options%20342e7860c6884b26bee739e1080aa558.md)

### Radio Button

- form:radiobutton

```html
Java <form:radiobutton path="favouriteLanguage" value="Java" />

<!-- Using list to store language list--> 
Favorite Language:
<form:radiobuttons path="favoriteLanguage" items="${student.favoriteLanguageOptions}"  />
```

### Check Box

- form:checkbox
- check multiple options → In this case, we need to add support when user selects multiple options, therefore, inside the java class, we need to have a collection to store any possible selection. i.e. Array of Strings.

```html
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

Operating System:

<ul>
	<c:forEach var="temp" items="${student.operatingSystems}">
		<li> ${temp} </li>
	
	</c:forEach>
</ul>
```

## Spring Validation

- A standard Bean Validation API
- Works for server side apps and client side JavaFX/Swing apps.
- Using Validation JAR files - download from Hibernate website, then add the files to the project.

    Paste to WEB-INF/lib

    ![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%205.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%205.png)

- Validation Feature
    - required
    - validate length
    - validate numbers
    - validate with regularexpressions
    - custom validation
- Validation Annotations

    ![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%206.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%206.png)

    - **@NotNull** – validates that the annotated property value is not null
    - **@AssertTrue** – validates that the annotated property value is *true*
    - **@Size** – validates that the annotated property value has a size between the attributes  and ; can be applied to ***String*, *Collection*, *Map*, and array properties**
    - **@Min** –Validates that the annotated property has a value no smaller than the attribute value
    - **@Max** – validates that the annotated property has a value no larger than the attribute value
    - ***@Email*** – validates that the annotated property is a valid email address

    Some annotations accept additional attributes, but the message attribute is common to all of them. This is the message that will usually be rendered when the value of the respective property fails validation.

    Some additional annotations that can be found in the JSR are:

    - ***@NotEmpty*** – validates that the property is not null or empty; can be applied to *String*, *Collection*, *Map* or *Array* values
    - ***@NotBlank*** – can be applied only to text values and validated that the property is not null or whitespace
    - ***@Positive*** and ***@PositiveOrZero*** – apply to numeric values and validate that they are strictly positive, or positive including 0
    - ***@Negative*** and ***@NegativeOrZero*** – apply to numeric values and validate that they are strictly negative, or negative including 0
    - ***@Past*** and ***@PastOrPresent*** – validate that a date value is in the past or the past including the present; can be applied to date types including those added in Java 8
    - ***@Future** and **@FutureOrPresent*** – validates that a date value is in the future, or in the future including the present

    ### Dealing with request.

    **@InitBinder** annotation works as a pre-processor

    It will pre-process each web request to our controller

    Method annotated with @InitBinder is executed

    For example: **Can use it to trim Strings (removing leading and trailing white spaces) → If string only has whitespace, trim it to null. Note: StringTrimmerEditor cannot deal with the whitespaces between two chars. It can only remove leading and trailing whitespaces.**

    File: XxxController.java

    ```java
    ...
    @InitBinder
    public void initBinder (WebDataBinder dataBinder) {

    	//true means trim the white sapce to null
    	StringTrimmerEditor stringTrimmerEditor = new StringTrimmerEditor(true);

    	// String.class determines all strings will be processed.
    	dataBinder.registerCustomEditor(String.class, stringTrimmerEditor);
    }
    ```

### How to add validation rules.

- Step 1: Add validation rules to the class
- Step 2: Display error message on HTML form
    - value determines rules, message determines the popup msg when there is error.
    - cssClass="error" → Display error message if set, need to code CSS codes to define the error class.

    ```html
    <form:errors path="LastName" cssClass="error" />
    ```

- Step 3: Perform validation in Controller class\
    - In the method signature, the BindingResult parameter must appear **immediately** after the model attribute.

    ![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%207.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%207.png)

- Step 4: Update confirmation page

### Number Range: @Min and @Max

```java
File: Customer.java	
@Min(value = 0, message = "Must be greater than or equal to zero.")
@Max(value = 10, message = "Must be smaller than or equal to 10.")
private int freePasses;
// add getter and setter

File: customer-form.jsp
Free spaces: <form:input path="freePasses" />
<form:errors path="freePasses" cssClass="error" />

// update customer-confirmation.jsp
```

### Make Integer Field Required

- refactored field: change "int" to "Integer"
- @Size not suitable.

### Handle String input for Integer Fields - **Custom Messages**

Step 1

- create a new folder under src. Right Click src → New → Others → Search for Folder → Name it resources.
- create a new .properties file in the resources folder. Right Click resources → New → Others → File → messages.properties
    - messages.properties location is very important. Must be under src/resources/messages.properties.
    - It is overriding the original error "code" with the own "message"

    ![Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%208.png](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Untitled%208.png)

- Step 2 - Load custom messages resource in Spring config file
    - Open WebContent/WEB-INF/spring-mvc-demo-servlet.xml

    ```xml
    <!-- Load custom message resources -->
    	<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    	
    		<property name="basenames" value="resources/messages" />
    		
    	</bean>
    ```

### Regular Expression

- Similar process like before

```java
File: Customer.java
@Pattern(regexp = "^[a-zA-Z0-9] {5}", message = "only 5 chars/digits")
private String postcode;

// add setter and getter

File: customer-form.jsp
Free spaces: <form:input path="freePasses" />
<form:errors path="freePasses" cssClass="error" />

// update customer-confirmation.jsp
```

## * Create a Custom Java Annotation

1. Create custom validation rule
    - Create a new sub-package under our main package
        - main package: com.allen.springdemo.mvc
        - sub-package: com.allen.springdemo.mvc.validation
            - New → Annotation
    - **Create @CourseCode annotation**
        - @Constraint() → Helper class that contains business rules /validation rules.
        - @Target() → can apply our annotation to a method or field
        - @Retention() → retain this annotation in the java class file. Process it at runtime.
    - Define the parameters of this annotation and assign a default value.
        - public String value() default "DEFAULT VALUE";
        - **if @CourseCode annotation doesn't introduce any parameter in the Customer.java, the default value set in the CourseCode.java will be used to validate the user input.**
    - Set up the default groups: can group related constraints {} → here is an empty collection
    - Set up the default payload: provide custom details about validation failure (severity level, error code etc.)

    File: CourseCode.java

    ```java
    package com.allen.springdemo.mvc.validation;

    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;

    import javax.validation.Constraint;
    import javax.validation.Payload;

    @Constraint(validateBy = CourseCodeConstraintValidator.class)
    @Target( { ElementType.METHOD, ElementType.Field } )
    @Retention(RetentionPolicy.RUNTIME)
    public @interface CourseCode {
    	
    	// define a value parameter and its default value
    	public String value() default "DEFAULT VALUE";

    	// define a message parameter and its default error message
    	public String message() default "DEFAULT ERROR MESSAGE";
      
    	// define  default groups
    	public Class<?>[] groups() default {};
    	
    	// define default payloads
    	public Class<? extends Payload>[] payload() default {};
      ....
    }
    ```

    - **Create CourseCodeConstraintValidator**
        - Created in the validation package
        - Helper class → Contains our custom business logic for validation
        - ConstraintValidator<OurAnnotation, ValidatingTargetType>
        - initialize(CourseCode theCourseCode) → the CourseCode.value() get the data from the value parameter.
        - isValid(String userEnteredData, ConstraintValidatorContext forAdditionalErrorMsg)
            - you can determine any business logic inside this method!!!
            - Talk to a database, call a web service etc.
            - simply return true and false.

        File: CourseCodeConstraintValidator.java

        ```java
        package com.allen.springdemo.mvc.validation;

        import javax.validation.ConstraintValidator;
        import javax.validation.ConstraintValidatorContext;

        public class CourseCodeConstraintValidator implements ConstraintValidator<CourseCode, String> {

        	private String coursePrefix;
        	
        	@Override
        	public void initialize(CourseCode theCourseCode) {
        		coursePrefix = theCourseCode.value();
        	}
        	
        	@Override
        	public boolean isValid(String theCode, ConstraintValidatorContext constraintValidatorContext) {
        		
        		boolean result;
        		
        		if (theCode != null) {
        			result = theCode.startsWith(coursePrefix);
        		}
        		else {
        			result = true;
        		}
        		
        		return result;
        	}
        }
        ```

    2.  Add Validation rule to Customer class

    3.  Display error messages on HTML form

    4.  Update confimation page

    [Integrate multiple validation string in one annotation.](Spring%20MVC%2039f90666d9274f5eb93f6592a86bf0d1/Integrate%20multiple%20validation%20string%20in%20one%20annota%204661a8f232254b31b3ca0c8916abe4b9.md)