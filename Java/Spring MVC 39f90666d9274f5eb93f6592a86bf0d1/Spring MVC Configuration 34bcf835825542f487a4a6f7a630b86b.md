# Spring MVC Configuration

### Step 1 : Configure Spring DispatcherServlet

![Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled.png](Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled.png)

- In web.xml file, add an entry for the spring DispatcherServlet or front controller
- tell where the configuration file located

### Step 2 : Set up URL mappings to Spring MVC Dispatcher Servlet

![Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%201.png](Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%201.png)

- tell the system what urls needs to be handled
- Note: mapping name should be matched the servlet name

### Step 3 : Add support for Spring component scanning

![Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%202.png](Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%202.png)

### Step 4 : Add support for conversion, formatting and validation

![Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%203.png](Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%203.png)

### Step 5 : Configure Spring MVC View Resolver

![Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%204.png](Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%204.png)

- how do we display these pages and where are the pages located
    - When your app provides a "view" name, Spring MVC will
        - prepend the prefix
        - append the suffix
        - tell where to look for the files to actually render your application

    ![Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%205.png](Spring%20MVC%20Configuration%2034bcf835825542f487a4a6f7a630b86b/Untitled%205.png)