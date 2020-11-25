# How To View Hibernate SQL Parameter Values

**Question:**

*I see hibernate printing out the query parameters as ? in the console.Is it possible to printout the value that was actually queried on thedatabase. Asking as this would help in the debugging purpose.*

Answer:

When using Hibernate, if you log the Hibernate SQL statements, you will see this:

Hibernate: insert into student (email, first_name, last_name, id) values (?, ?, ?, ?)

However, for debugging your application, you want to see the actual parameter values in the Hibernate logs. Basically, you want to get rid of the question marks in the Hibernate logs.

You can view the actual parameters by viewing the low-level trace of the Hibernate logs. This is not set up by default. However, we can add log4j to allow us to see these low-level logs.

# **Here is an overview of the process:**

1. Add log4j to your project classpath

2. Add log4j.properties to your “src” directory

# **Here are the detailed steps:**

**1. Add log4j to your project classpath**

1a. Download log4j v1.2.17 from this link: – [http://central.maven.org/maven2/log4j/log4j/1.2.17/log4j-1.2.17.jar](http://central.maven.org/maven2/log4j/log4j/1.2.17/log4j-1.2.17.jar)

1b. Copy this file to your project’s lib directory

![https://i.udemycdn.com/redactor/2016-10-06_12-43-18-43665fab7523d5393142fa06db76c540/image1.png](https://i.udemycdn.com/redactor/2016-10-06_12-43-18-43665fab7523d5393142fa06db76c540/image1.png)

1c. Right-click your Eclipse project and select **Properties**

1d. Select **Build Path > Libraries > Add JARS…**

1e. Select the **log4j-1.2.17.jar** file from the **lib** directory

![https://i.udemycdn.com/redactor/2016-10-06_12-43-33-16c189ccb8a5d926415b9fcaa99995c2/image2.png](https://i.udemycdn.com/redactor/2016-10-06_12-43-33-16c189ccb8a5d926415b9fcaa99995c2/image2.png)

**2. Add log4j.properties to your “src” directory**

2a. Copy the text from below

# Root logger optionlog4j.rootLogger=DEBUG, stdout # Redirect log messages to consolelog4j.appender.stdout=org.apache.log4j.ConsoleAppenderlog4j.appender.stdout.Target=System.outlog4j.appender.stdout.layout=org.apache.log4j.PatternLayoutlog4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n log4j.logger.org.hibernate=TRACE

2b. Save this file as "log4j.properties" in your “src” directory

![https://i.udemycdn.com/redactor/2016-10-06_12-44-00-feca336a1d4990fe31e1612c44da03bd/image3.png](https://i.udemycdn.com/redactor/2016-10-06_12-44-00-feca336a1d4990fe31e1612c44da03bd/image3.png)

Note: This file has an important setting:

`log4j.logger.org.hibernate=TRACE` 

This allows you see a low-level trace of Hibernate and this allows you see the real SQL parameter values.

Now run your application. You will see a lot of low-level TRACE logs in the Eclipse Console window.

Right-click in the Eclipse Console window and select **Find/Replace…**

Search for: **binding parameter**

or search for: **extracted value**

(the search string changes depending on which version of Hibernate you are using)

You will see the logs with the real parameter values. Congrats!

![https://i.udemycdn.com/redactor/2016-10-06_12-44-20-3b68464e433153ad29cb254bb1f7f136/image4.png](https://i.udemycdn.com/redactor/2016-10-06_12-44-20-3b68464e433153ad29cb254bb1f7f136/image4.png)