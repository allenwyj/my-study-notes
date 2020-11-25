# Deploying your App to Tomcat as a Web Application Archive (WAR) file

**Bonus: Deploying your App to Tomcat as a Web Application Archive (WAR) file**

When you deploy your Java web apps, you can make use of a Web Application Archive (WAR) file.

The Web Application Archive (WAR) file is a compressed version of your web application. It uses the zip file format but the file has the .war extension.

If you are using Eclipse, then the best way to visualize it is think of your "WebContent" directory being compressed as a zip file with the .war extension.

This includes all of your web pages, images, css etc. It also includes the WEB-INF directory which includes your classes in WEB-INF/classes and supporting JAR files in WEB-INF/lib.

The WAR file format is part of the Java EE / Servlet specification. As a result, all Java EE servers support this format (ie jboss, weblogic, websphere, glassfish and tomcat).

Below, I provide steps on how to create a WAR file in Eclipse. I also show how to deploy the WAR file on Tomcat.

---

1. In Eclipse, stop Tomcat

2. Right-click your project and select Export > WAR File

3. In the Destination field, enter: <any-directory>/mycoolapp.war

4. Outside of Eclipse, start Tomcat- If you are using MS Windows, then you should find it on the Start menu

5. Make sure Tomcat is up and running by visiting: http://localhost:8080

6. Deploy your new WAR file by copying it to <tomcat-install-directory>\webapps

Give it about 10-15 seconds to make the deployment. You'll know the deployment is over because you'll see a new folder created in webapps ... with your WAR file name.

7. Visit your new app. If your war file was: mycoolapp.war then you can access it with: http://localhost:8080/mycoolapp/