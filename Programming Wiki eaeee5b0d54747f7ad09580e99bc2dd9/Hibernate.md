# Hibernate

### A framework for persisting / saving Java objects in a database.

---

## Benefits

- Hibernate handles all of the low-level SQL
- Minises the amount of JDBC code you have to develop
    - i.e. setting up connection, SQL query,
- Hibernate provides the Object-to-Relational Mapping (ORM)
    - Defining mapping between Java Class and a Database Table through Hibernate.

# Hibernate and JDBC

- Hibernate uses JDBC for all database communications. (Hibernate API)

    [How To View Hibernate SQL Parameter Values](Hibernate%2053255e36e52e41df9023fef909196039/How%20To%20View%20Hibernate%20SQL%20Parameter%20Values%207e175743a14a4c49b9b8f3d42627aac1.md)

    [How to read Dates with Hibernate](Hibernate%2053255e36e52e41df9023fef909196039/How%20to%20read%20Dates%20with%20Hibernate%20353a8da79949443aba2588ed6b032b72.md)

# MySQL

- use root user to create MySQL user for the application (it depends), then use the new user to create a new connection and login the database

    [01-create-user.sql](Hibernate%2053255e36e52e41df9023fef909196039/01-create-user.sql)

- create a table in the database

    [02-student-tracker.sql](Hibernate%2053255e36e52e41df9023fef909196039/02-student-tracker.sql)

## Generate ERD in MySQL

- Select Database → Reverse Engineer → Select connection → Select the schema → Tick 'Import MySQL Table Objects' and 'Place imported objects on a diagram'

# Hibernate Development Environment Overview

- Overview
    - setup Hibernate and JDBC Driver (JAR files)
    - Configure the hibernate.cfg.xml file
    - map classes to the database
    - create main class

## Setting up the environment

1. IDE - eclipse
2. Database Server
    1. Download mysql community server and mysql workbench
    2. connecting via terminal: mysql -u root -p, password: koutianwu

        [https://www.youtube.com/watch?v=UcpHkYfWarM](https://www.youtube.com/watch?v=UcpHkYfWarM)

    3. connecting via mysql workbench.
3. Hibernate JAR files and JDBC Driver - setting up Hibernate in eclipse .. ***Build path***
    1. Create Eclipse Project
        - change eclipse prospective: Window → Perspective → Open Perspective → Java
        - create a new java project.
    2. Create a new folder under the project folder → lib
    3. Copy and paste the source files from Hibernate. (Hibernate folder → lib → required → all files) to project/lib
    4. Download files for JDBC Driver
        - MySQL Community → Connectors/J → select 'Platform Independent' → download the zip → copy 'mysql-connector-java-8.0.20.jar' → paste to project/lib/
    5. Add these JAR files to the project's classpath : Right-click project → Properties
    6. Create a new main method to test the connection.

    ![Hibernate%2053255e36e52e41df9023fef909196039/Untitled.png](Hibernate%2053255e36e52e41df9023fef909196039/Untitled.png)

    ```java
    File: TestJdbc.java // use to test the connection with the db
    package com.allen.jdbc;

    import java.sql.Connection;
    import java.sql.DriverManager;

    public class TestJdbc {

    	public static void main(String[] args) {
    		
    		// Update for MySQL 8
    		String jdbcUrl = "jdbc:mysql://localhost:3306/hb_student_tracker?useSSL=false&serverTimezone=UTC";
    		String user = "hbstudent";
    		String password = "hbstudent";
    		
    		try {
    			System.out.println("Connecting to database " + jdbcUrl);
    			
    			Connection myConnection = DriverManager.getConnection(jdbcUrl, user, password);
    			
    			System.out.println("Connection successful!!!");
    		}
    		catch (Exception e) {
    			e.printStackTrace();
    		}
    	}
    }
    ```

# Hibernate Configuration with Annotations

## Creating the Hibernate Configuration File

- copy and paste the baisc hibernate configuration file in root of "src" dir
    - Change the connection url.

    File: **project/scr/hibernate.cfg.xml**

    ```xml
    <!DOCTYPE hibernate-configuration PUBLIC
            "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
            "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

    <hibernate-configuration>

        <session-factory>

            <!-- JDBC Database connection settings -->
            <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>
            <property name="connection.url">jdbc:mysql://localhost:3306/hb_student_tracker?useSSL=false&amp;serverTimezone=UTC</property>
            <property name="connection.username">hbstudent</property>
            <property name="connection.password">hbstudent</property>

            <!-- JDBC connection pool settings ... using built-in test pool -->
            <property name="connection.pool_size">1</property>

            <!-- Select our SQL dialect -->
            <property name="dialect">org.hibernate.dialect.MySQLDialect</property>

            <!-- Echo the SQL to stdout -->
            <property name="show_sql">true</property>

    		<!-- Set the current session context -->
    		<property name="current_session_context_class">thread</property>
     
        </session-factory>

    </hibernate-configuration>
    ```

## Map class to  database table and map fields to database columns

- Choose javax.persistence package for the imports - standard interface hibernate implements
- // annotate the class as an entity and map to db table
- // define the fields
- // annotate the fields with db column names
- // create constructors
- // getter/setter
- // generate toString() method

```java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="student")
public class Student {

	@Id //determine the pk
  @GeneratedValue(strategy=GenerationType.IDENTITY) //AUTO_INCREMENT
	@Column(name="id")
	private int id;

	@Column(name="first_name")
	private String firstName;
	
	...

	// generate the default constructor
	// generate the constructor with fields(not including pk)
	// getters and setters for all fields.

	// This method is used to show the detail of an object
	@Override
	public String toString() {
		return "Student [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", email=" + email
				+ "]";
	}

}
```

### Hibernate Identity - Primary Key

- If not using specific code, hibernate will use the default strategy to generate the pk
    - **It will only work while creating one new object and saving it to the db**
    - **If creating multiple objects at the same time, we need to generate different Id for them**
- If using `@GenerateValue(...)` after `@Id`, it can specify the strategy that will be used for generating the new row.

    ![Hibernate%2053255e36e52e41df9023fef909196039/Untitled%201.png](Hibernate%2053255e36e52e41df9023fef909196039/Untitled%201.png)

    ```java
    	@Id //determine the pk
    	@GeneratedValue(strategy=GenerationType.IDENTITY) //AUTO_INCREMENT
    	@Column(name="id")
    	private int id;
    ```

- Can create the custom generation strategy

[Two Key Players](Hibernate%2053255e36e52e41df9023fef909196039/Two%20Key%20Players%20fc139533cf0b4af9a514f8001143b32e.csv)

## Setting up SessionFactory and create a session to perform CRUD

```java
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

import com.allen.hibernate.demo.entity.Student;

public class CreateStudentDemo {

	public static void main(String[] args) {
		
		**// Create session factory**
		// If we have more class, we need to add them into annotated class.
		SessionFactory factory = new Configuration()
														.configure("hibernate.cfg.xml")
														.addAnnotatedClass(Student.class)
														.buildSessionFactory();
		
		// Create session
		Session session = factory.getCurrentSession();
		
		try {			
			// create a student object
			Student tempStudent = new Student("Allen", "Wu", "allenwuyujie@gmail.com");
			
			// start a transaction
			session.beginTransaction();
			
			// save the student object
			session.save(tempStudent);
			
			// commit the transaction
			session.getTransaction().commit();
			
		}
		finally {
			factory.close();
		}
	}
}
```

## Hibernate CRUD Apps

- Create, Read, Update, Delete objects

### C&R: Saving / retrieving a Java Object with Hibernate

- When an object is saved to the db, the pk (here is the id) won't be created until it is saving to the database.

```java
// create Java object
Student theStudent = new Student("Allen", "Wu", "allen@gmail.com");

// save it to database
int theId = (Integer) session.save(theStudent);

// Read
// retrieve data from database using the primary key
Student myStudent = session.get(Student.class, theId);

// =====================================================
// find out the student's id: pk
System.out.println("Saved student. Generated id: " + tempStudent.getId());

// now get a new session and start transaction
session = factory.getCurrentSession();
session.beginTransaction();

// retrieve student based on the id: pk
System.out.println("\nGetting student with id: " + tempStudent.getId());

Student myStudent = session.get(Student.class, tempStudent.getId());

System.out.println("Get complete: " + myStudent);

// commit the transaction
session.getTransaction().commit();

System.out.println("Done!");
```

### R: Querying for Java Objects - **Hibernate Query Language (HQL)**

- similar in nature to SQL
- Use the Java property name (not column name) i.e. s.lastName

The Query interface defines two methods for running SELECT queries:

- `Query.getSingleResult` - Execute a SELECT query that returns a single untyped result. For use when exactly one result object is expected.
- `Query.getResultList` - Execute a SELECT query and return the query results as an untyped List. For general use in any other case.

```java
**import org.hibernate.query.Query;**

// Retrieving All students
Query query = session.createQuery("from Student");

// Retrieving Students: lastName = 'Doe'
Query query = session.createQuery("from Student s where s.lastName='Doe'")

// Retrieving Students using OR predicate
Query query = session.createQuery("from Student s where s.lastName='Doe'"
																+ " OR s.firstName='Daffy'");

// Retrieving Students using LIKEpredicate
Query query = session.createQuery("from Student s where"
																+ " s.firstName LIKE 'Daffy'");

// query students: where email ends with '%gmail.com' -> % ends with
List<Student> theStudents = session.createQuery("from Student s where"
				+ " s.email LIKE '%gmail.com'").getResultList();

// return a list of students from the database
List<Student> students = query.getResultList();

session.getTransaction().commit();

```

### U: Update an object in the database

- Method 1: get the object by querying its pk, then update the value by calling setters (not commonly used). The updated student is saved when we commit the transaction to the database.
    - The update is monitored without manually using update method due to the object is in Persistent state.
- Method 2: execute update : update the object into the database

```java
try {
		// Method 1:
		int studentId = 1;
		
		// now get a new session and start transaction
		session = factory.getCurrentSession();
		session.beginTransaction();
		
		Student theStudent = session.get(Student.class, studentId);
		theStudent.setFirstName("myUpdateName");
		
		// commit the transaction
		session.getTransaction().commit();
		
		// ======================== I'm a divider ================================
		// Method 2
		session = factory.getCurrentSession();
		session.beginTransaction();
		
		// update email by applying a specific rule
		session.createQuery("update Student s set s.email='abc@hotmail.com'" + 
					" where s.email='test@hotmail.com'").executeUpdate();
		
		// BULK UPDATE : update email for all students.
		session.createQuery("update Student set email='abc@hotmail.com'").executeUpdate();
		
		session.getTransaction().commit();
}
finally {
	factory.close();
}
```

### D: Delete an Object from the database

```java
try {
		**// Method 1:**
		int studentId = 1;
		
		// now get a new session and start transaction
		session = factory.getCurrentSession();
		session.beginTransaction();
		
		Student theStudent = session.get(Student.class, studentId);
		
		// delete the student
		session.delete(theStudent);
		
		// commit the transaction
		session.getTransaction().commit();

		// ======================== I'm a divider ================================
		**// Method 2**
		session = factory.getCurrentSession();
		session.beginTransaction();
		
		// delete student
		**session.createQuery("delete Student where email='test@hotmail.com'")
						.executeUpdate();**
		
		session.getTransaction().commit();
}
```

# Hibernate Advanced Mappings

## Entity Lifecycle

- help to prevent from having stale data in the memory, which is different from data in the actual database.

![Hibernate%2053255e36e52e41df9023fef909196039/Untitled%202.png](Hibernate%2053255e36e52e41df9023fef909196039/Untitled%202.png)

**Transient state**: An object is in transient state if it just has been instantiated using the new operator and there is no reference of it in the database i.e it does not represent any row in the database.

**Persistent state**: An object is in the persistent state if it has some reference in the database i.e it represent some row in the database and identifier value is assigned to it. If any changes are made to the object then hibernate will detect those changes and effects will be there in the database that is why the name Persistent. These changes are made when session is closed. A persistent object is in the session scope.

**Detached state:** An object that has been persistent and is no longer in the session scope. The hibernate will not detect any changes made to this object. It can be connected to the session again to make it persistent again. 
临时(Transient) - 由new操作符创建，且尚未与Hibernate Session 关联的对象被认定为临时(Transient)的。临时(Transient)对象不会被持久化到数据库中，也不会被赋予持久化标识(identifier)。 如果临时(Transient)对象在程序中没有被引用，它会被垃圾回收器(garbage collector)销毁。 使用Hibernate Session可以将其变为持久(Persistent)状态。(Hibernate会自动执行必要的SQL语句) 

持久(Persistent) - 持久(Persistent)的实例在数据库中有对应的记录，并拥有一个持久化标识(identifier)。 持久(Persistent)的实例可能是刚被保存的，或刚被加载的，无论哪一种，按定义，它存在于相关联的Session作用范围内。 Hibernate会检测到处于持久(Persistent)状态的对象的任何改动，在当前操作单元(unit of work)执行完毕时将对象数据(state)与数据库同步(synchronise)。 开发者不需要手动执行UPDATE。将对象从持久(Persistent)状态变成瞬时(Transient)状态同样也不需要手动执行DELETE语句。 

游离(Detached) - 与持久(Persistent)对象关联的Session被关闭后，对象就变为游离(Detached)的。 对游离(Detached)对象的引用依然有效，对象可继续被修改。游离(Detached)对象如果重新关联到某个新的Session上， 会再次转变为持久(Persistent)的(在Detached其间的改动将被持久化到数据库)。 这个功能使得一种编程模型，即中间会给用户思考时间(user think-time)的长时间运行的操作单元(unit of work)的编程模型成为可能。 我们称之为应用程序事务，即从用户观点看是一个操作单元(unit of work)。

- sync the data from the database → **refresh**
- if we have a persistent object, we do a **commit/rollback/close** → the object is detached
    - detached object is not associated with a given Hibernate session
- if we want to reattach a detached object to the Hibernate session, we simply get a reference to that object and do a **merge** on it → merge
- if we have a persistent object, we do a **delete** or **remove**, then the object is in the remove state → delete/remove
    - we can call **persist** or **rollback** to that object to make it back to the managed state.
    - if we use **commit** on a removed object, it's removed from the database, and it's just a transient state.

![Hibernate%2053255e36e52e41df9023fef909196039/Untitled%203.png](Hibernate%2053255e36e52e41df9023fef909196039/Untitled%203.png)

## Cascade

- By default, no operations are cascaded

[save() and persist() - parent/child](Hibernate%2053255e36e52e41df9023fef909196039/save()%20and%20persist()%20-%20parent%20child%2078b01de561cc40e4ba7bd5c91e69fc83.md)

# Fetch Types : Eager vs Lazy Loading

Best Practice: Only load data when absolutely needed, *Prefer Lazy loading instead of Eager loading

- Eager will retrieve everything
- Lazy will retrieve on request

## Eager Loading

- It will load the main entity and all dependent entities
    - For example, load all instructors and all of their courses at once, when the below codes run:

        `List<Instructor> instructorList = session.createQuery("from instructor").getResultList();`

    - It will affect the performance if the data is huge
- Even though we search by using keyword, it still loads others

## Lazy Loading - Good Practice

- It will load the main entity first, then load dependent entities on demand.
    - For example, Hibernate will load the all instructors data but it is not going to load the Course data for the loaded instructor until `tempInstructor.getCourse()` method called.
    - Generally, Hibernate will ask the Course table, only when you access the data.
- **Requires an open Hibernate session - need an connection to database to retrieve data.**
    - if the session is closed, Hibernate will throw an exception.
    - If the data is retrieved before session closed, it will be stored in the memory, and it still can be accessed after the session closed.

    [The best way to handle the LazyInitializationException - Vlad Mihalcea](https://vladmihalcea.com/the-best-way-to-handle-the-lazyinitializationexception/)

- The most common methods:
    - Option 1: session.get and call appropriate getter method(s)
        - Load the data on demand in advance (in memory), so it can be used after the session closed.
    - Option 2: Hibernate query with HQL
        - Using `JOIN FETCH` to fetch the specific instructor and courses at once

        ```java
        // get the instructor from db
        int theId = 1;
        Query<Instructor> query = session.createQuery("select i from Instructor i "
        													+ "JOIN FETCH i.courses "
        													+ "where i.id = :theInstructorId",
        													Instructor.class);

        // set parameter on query :theInstructorId
        query.setParameter("theInstructorId", theId);

        // execute query and get instructor, 
        // load instructor and courses all at once.
        Instructor tempInstructor = query.getSingleResult(); 
        ```

    [How to solve the "failed to lazily initialize a collection of role" Hibernate exception](https://stackoverflow.com/a/51055820/1998950)

### Fetch Type For Different Mappings

- It can be defined when we define the mapping relationship.

```java
public class Instructor {
	@OneToMany(fetch=FetchType.LAZY, mappedBy="instructor")
	private List<Course> courses;
```

- by default, for different relationships, the fetch types are different - but it can be overridden (explicitly defining).

    ![Hibernate%2053255e36e52e41df9023fef909196039/Untitled%204.png](Hibernate%2053255e36e52e41df9023fef909196039/Untitled%204.png)

## @JoinColumn

- Mapping the foreign key ( having the FK in the database)
    - If the join is for a OneToOne or ManyToOne mapping **using a foreign key** mapping strategy, the foreign key column is in the table of **the source entity** or embeddable.
        - FK is in the source class, and @JoinColumn(name="") is in the source class.
    - If the join is for a unidirectional OneToMany mapping **using a foreign key** mapping strategy, the foreign key is in the table of the **target entity.**
        - FK is in the target class, but the @JoinColumn is in the source class.
    - If the join is for a ManyToMany mapping or for a OneToOne or bidirectional ManyToOne/OneToMany mapping **using a join table**, the foreign key is in a join table.
    - If the join is for an element collection, the foreign key is in a collection table.
- **If uni-directional, normally the @JoinColumn will define the FK in the parent class, no matter where the FK is in the database, if bi-directional, normally it exists in the many side.**

# One-to-one Mapping

- setup the database

    [create-db.sql](Hibernate%2053255e36e52e41df9023fef909196039/create-db.sql)

- test the JDBC connection by using the new database name
- setup JDBC URL in Hibernate config file
- create entity classes
    - **instructor_detail_id is defined in instructor table as a FK**. In database, foreign key is configured to reference the ID column in instructor_detail table.
    - Containing another Class Object as a field to get ready to setting up the FK. (in Instructor.java)
    - `@OneToOne` → mapping relationship
    - `@JoinColumn` → mapping the foreign key
- setup the main class
    - When setting up the session factory, the **`.addAnnotatedClass()`** needs to pass **all classes** which will be used in this main class.

### **Uni-Directional One-To-One Mapping**

- one way A→ B, can't trace back

```java
*File: Instructor.java*
@Entity
@Table(name = "instructor")
public class Instructor {

	**@OneToOne(cascade = CascadeType.ALL)
	@JoinColumn(name = "instructor_detail_id")** **// the foreign key name in the db**
	private InstructorDetail instructorDetail;

	...

}

*File: CreateDemo.java*
public class CreateDemo {
	public static void main(String[] args) {
		...
		try {	
			// create the objects
			Instructor tempInstructor = new Instructor("Allen", "Wu", "allen@gmail.com");
			InstructorDetail tempInstructorDetail = new InstructorDetail("allen.com/youtube", "love to code!!!!");
			
			**// associate the objects
			tempInstructor.setInstructorDetail(tempInstructorDetail);**
			
			// start a transaction
			session.beginTransaction();
			
			**// save the instructor
			//
			// Note: this will ALSO save the details object
			// because of CascadeType.ALL
			//**
			session.save(tempInstructor);
			
			**// delete the instructor - need to check if not null**
			if (tempInstructor != null) {
				**//**
				**// Note: will ALSO delete associated "details" object
				// because of CascadType.ALL
				//**
				session.delete(tempInstructor);
			}

			// commit the transaction
			session.getTransaction().commit();
			
		}
		...
	}
}
```

### **Bi-Directional One-To-One Mapping**

- Instructor ↔ InstructorDetail : Starts with Instructor and make it back to the InstructorDetail
    - Instructor has a FK

        ```java
        public class Instructor {
        	
        	**@OneToOne(cascade = CascadeType.ALL)
        	@JoinColumn(name = "instructor_detail_id")** **// the foreign key in the db**
        	private InstructorDetail instructorDetail;
        ```

- **Benefit**: Keep the existing database schema
    - no changes required to database
    - **simply update java class —> means we don't need to configure the database**
- each class needs to have a field for another class object.
- In InstructorDetail class,

    `@OneToOne(mappedBy="instructorDetail", cascade=CascadeType.ALL)`, it means:

    - 已经做好mapping了，
    - Look at the instructorDetail property in the Instructor class
    - Use information from the Instructor class  `@JoinColumn`
        - we don't need to use `@JoinColumn` in InstructorDetail class, and also we don't need to create a new column for 'instructor_detail' table in the database
        - we just need to configure InstructorDetail java class to add a field (

            `private Instructor instructor`) and **use getter** to get Instructor's information.

    - To help find associated instructor

**NOTE: Instructor class needs to be configured by `@JoinColumn()` (same as Uni-Directional)**

```java
*File: InstructorDetail.java
public class InstructorDetail {

	**// add new field for instructor (also add getter/setter)
	// add @OneToOne annotation
	@OneToOne(mappedBy = "instructorDetail", cascade = CascadeType.ALL)
	private Instructor instructor;**
}*
```

- Cascade delete will work on both classes in the database depends on cascade type.
    - Delete Class B will cause the deletion of Class A if Class B has `cascadeType.ALL`
    - In the below case, the cascade types don't include remove, so deleting Class B will not cascade to the associated instructor (Class A)
        - we also need to remove the associated object reference in the main class.

        ```java
        *File: InstructorDetail.java*
        @OneToOne(mappedBy = "instructorDetail", 
        			cascade = {CascadeType.DETACH, CascadeType.MERGE, 
        									CascadeType.PERSIST,CascadeType.REFRESH})
        private Instructor instructor;

        *File: MainClass.java
        ...
        			**// remove the assoicated object ref
        			// break bi-directional link**
        			**tempInstructorDetail.getInstructor().setInstructorDetail(null);**
        			session.delete(tempInstructorDetail);
        ...*
        ```

### *Exception Handling

- If the object gets back is null, there will be a leaking problem, because the session doesn't close
    - `try{} finally{}` block try block gets error and doesn't proceed commit and
    - therefore, we can add a catch block to handle the exception and close the session in the finally block.

```java
		try {			
			...
			// getting object from the database, may retrieve nothing
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			// handle connection leak issue
			session.close();
			factory.close();
		}
```

# One-To-Many / Many-To-One Mapping

### Bi-directional One-To-Many / Many-To-One Mapping

- Real-World Project Requirement: deleting one won't delete another one ( DO NOT apply cascade delete)
1. Define database tables - Instructor has many Courses.
    - Course table has a FK instructor_id
    - `UNIQUE KEY `TITLE_UNIQUE` (`TITLE`)` - Prevent duplicate course titles

        [create-db.sql](Hibernate%2053255e36e52e41df9023fef909196039/create-db%201.sql)

2. Create entity class (the Many side)
    - In the many side, we will store a FK from the one side in the database. So we need to define the relationship and map the foreign key.
    - `@JoinColumn`
    - `@ManyToOne`
    - Add support for Cascading - DO NOT apply cascading deletes

    ```java
    public class Course {
    	...

    	@ManyToOne(cascade = {CascadeType.DETACH, CascadeType.MERGE, 
    													CascadeType.PERSIST,CascadeType.REFRESH})
    	@JoinColumn(name="instructor_id")
    	private Instructor instructor;
    ```

3. Create entity class (the One side)
    - a LIST field in the java class, but no need to create a new column in the database.
    - `@OneToMany(mappedBy='instructor')`
    - **mappedBy** tells Hibernate
        - Look at the instructor property in the Course class
        - Use information from the Course class `@JoinColumn`
        - To help find associated courses for instructor
    - Add support for Cascading - DO NOT apply cascading deletes

    ```java
    public class Instructor {
    	...

    	@OneToMany(mappedBy="instructor", 
    							cascade = {CascadeType.DETACH, CascadeType.MERGE, 
    													CascadeType.PERSIST,CascadeType.REFRESH})
    	private List<Course> courses;
    ```

4. **Add convenience methods in the One side for bi-directional**

    First we check if our list of courses is null -- if so, we create a new ArrayList to hold courses.

    Then we add the course to the array list.  So now the Instructor object 'knows' what courses it has.

    Finally, we need to call the 'setInstructor' method on the course we are adding.  We pass it this ('this' is a reference to the current object, Instructor) so that the course object will have a link back to the Instructor.  In other words, the course object now knows which instructor it is associated with.

    - The order doesn't matter. `tempCourse.setInstructor(this);` will still point to the same object (tempCourse) as `courses.add(tempCourse);`

    ```java
    public class Instructor {
    	@OneToMany(mappedBy="instructor",
    			   cascade= {CascadeType.PERSIST, CascadeType.MERGE,
    						 CascadeType.DETACH, CascadeType.REFRESH})
    	private List<Course> courses;	

    	// add convenience methods for bi-directional relationship
    	public void addCourse(Course tempCourse) {
    		if (courses == null) {
    			courses = new ArrayList<>();
    		}
    		
    		courses.add(tempCourse);
    		tempCourse.setInstructor(this);
    	}
    ```

5. Create Main App
    - Save courses through Instructor

        [](https://www.udemy.com/course/spring-hibernate-tutorial/learn/lecture/12128430#questions/3189118)

        ```java
        try {			
        			// start a transaction
        			session.beginTransaction();
        			
        			// get the instructor from db
        			int theId = 1;
        			Instructor tempInstructor = session.get(Instructor.class, theId);
        			
        			// create some courses
        			Course tempCourse1 = new Course("Teaching English");
        			Course tempCourse2 = new Course("Teaching French");
        			
        			// add courses to instructor
        			tempInstructor.addCourse(tempCourse1);
        			tempInstructor.addCourse(tempCourse2);
        			
        			// save the courses
        			session.save(tempCourse1);
        			session.save(tempCourse2);
        			
        			**// or we can use session.persist() to save the courses
        			// session.persist(tempInstructor);**			

        			// commit the transaction
        			session.getTransaction().commit();
        		}
        ```

    - Get courses through instructor

        ```java
        try {			
        			// start a transaction
        			session.beginTransaction();
        			
        			// get the instructor from db
        			int theId = 1;
        			Instructor tempInstructor = session.get(Instructor.class, theId);
        			
        			System.out.println("Instructor: " + tempInstructor);
        			
        			// get course for the instructor
        			System.out.println("Course: " + tempInstructor.getCourses());
        			
        			
        			// commit the transaction
        			session.getTransaction().commit();
        			
        			System.out.println("Done!");
        		}
        ```

    - Delete course

        ```java
        try {			
        			// start a transaction
        			session.beginTransaction();
        			
        			// get a course
        			int theCourseIde = 10;
        			Course tempCourse = session.get(Course.class, theCourseIde);
        			
        			// delete the course
        			System.out.println("Deleting course: " + tempCourse);
        			session.delete(tempCourse);
        			
        			// commit the transaction
        			session.getTransaction().commit();
        		}
        ```

    ---

### Uni-directional One-To-Many / Many-To-One Mapping

- A Class → B Class, B can't trace back to A - Course has many Reviews

```java
public class Course {
	...
	@OneToMany(fetch=FetchType.LAZY, cascade=Cascade.ALL)
	@JoinColumn(name="course_id")
	private List<Review> reviews;
```

- In this scenario, @JoinColumn tells Hibernate
    - Look at the course_id column in the review table
    - Use this information to help find associated reviews for a course
- In the database, FK exists in the Many side, but @JoinColumn exists in the One side class. Because we want the One side can trace the data from the Many side.
- Development Process
    1. Step 1 - Prepare the database
        - Review table has a FK course_id

        [create-db.sql](Hibernate%2053255e36e52e41df9023fef909196039/create-db%202.sql)

    2. Step 2 - Create the entity classes
        - Only the class A will have @JoinColumn to trace the data from the target entity's FK,  because it's uni-directional.
    3. Similar steps for the rest.

# Many-To-Many Mapping

- Keep track of relationships - Need to track which student is in which course.
- Therefore, we need to use a Join Table to perform this task
- Join Table
    - A table that provides a mapping between two tables.
    - It has foreign keys for each table to define the mapping relationship.
- Development Steps:
    1. Define database tables

        [create-db.sql](Hibernate%2053255e36e52e41df9023fef909196039/create-db%203.sql)

    2. Develop entity classes
        - Add @ManyToMany annotation
        - Add @JoinTable annotation - For both classes
            - It tells Hibernate - From the course side.
                - Look at the course_id column in the course_student table
                - For other side (inverse), look at the student_id column in the course_student table
                - Use this information to find relationship between course and students.
        - For the inverse side (the other side), implement the above steps but change the inverse column.
        - **Note: Do Not apply cascade delete.**

        ```java
        **File: Course.java**
        public class Course {
        	...
        	@ManyToMany(fetch=FetchType.LAZY,
        				cascade= {CascadeType.PERSIST, CascadeType.MERGE,
        				 CascadeType.DETACH, CascadeType.REFRESH})
        	@JoinTable(
        			name="course_student",
        			joinColumns=@JoinColumn(name="course_id"),
        			inverseJoinColumns=@JoinColumn(name="student_id")
        			)	
        	private List<Student> students;
        	...
        }

        **File: Student.java**
        public class Student {
        	...
        	@ManyToMany(fetch=FetchType.LAZY,
        				cascade= {CascadeType.PERSIST, CascadeType.MERGE,
        				 CascadeType.DETACH, CascadeType.REFRESH})
        	@JoinTable(
        			name="course_student",
        			joinColumns=@JoinColumn(name="student_id"),
        			inverseJoinColumns=@JoinColumn(name="course_id")
        			)	
        	private List<Course> courses;
        	...
        }
        ```

    3. Create Main App

        ```java
        **// Adding more students in one course**
        try {			
        			// start a transaction
        			session.beginTransaction();
        			
        			// create a course
        			Course tempCourse = new Course("Pacman - How To Score One Million Points");
        			
        			// save the course
        			System.out.println("\nSaving the course...");
        			session.save(tempCourse);
        			System.out.println("saved the course: " + tempCourse);
        			
        			// create the students
        			Student tempStudent1 = new Student("A", "b", "A@b.com");
        			Student tempStudent2 = new Student("C", "d", "C@d.com");
        			
        			// add students to the course
        			tempCourse.addStudent(tempStudent1);
        			tempCourse.addStudent(tempStudent2);
        			
        			// save the students
        			System.out.println("\nsaving the students");
        			session.save(tempStudent1);
        			session.save(tempStudent2);
        			
        			// commit the transaction
        			session.getTransaction().commit();
        			
        			System.out.println("Done!");
        			
        		}

        **// Adding more courses in one student**
        try {			
        			
        			// start a transaction
        			session.beginTransaction();
        			
        			// get the student 1 from database
        			int theId = 2;
        			Student tempStudent = session.get(Student.class, theId);
        			
        			System.out.println("\n Loaded student: " + tempStudent);
        			System.out.println("courses: " + tempStudent.getCourses());
        			
        			// create more courses
        			Course course1 = new Course("Programming in Java");
        			Course course2 = new Course("Intro to database");
        			
        			// add the student to courses
        			course1.addStudent(tempStudent);
        			course2.addStudent(tempStudent);
        			
        			// save the courses
        			System.out.println("\nSaving the courses,,,");
        			
        			session.save(course1);
        			session.save(course2);
        			
        			// commit the transaction
        			session.getTransaction().commit();
        			
        			System.out.println("Done!");
        }
        ```