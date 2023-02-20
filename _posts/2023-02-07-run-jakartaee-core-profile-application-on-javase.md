---
layout: post
title: "Run a Jakarta EE Core profile application on Java SE"
date: 2023-02-20 
---

In this blog post, I want to explore running a Jakarta EE Core profile application on Java SE without an application server.
Java/Jakarta Experts - Markus Karg, Rudy De Busscher and Francesco Marchioni have already blogged and demonstrated how to achieve 
this on their blog respectively at https://headcrashing.wordpress.com/tag/java-se-bootstrap-api/, https://www.atbash.be/2023/01/05/run-your-jakarta-application-without-runtime/ 
and http://www.mastertheboss.com/jboss-frameworks/resteasy/getting-started-with-jakarta-restful-services/. I wanted to take
this further and explore the possibilities/ways to create a Jakarta REST CRUD application. This project is build using Netbeans, Maven and
the Jakarta EE core profile.

The Jakarta RESTful web service API built in this tutorial is for CRUD Operations (Create, Read, Update, Delete) which corresponds to the standard
HTTP methods (POST, GET, PUT, DELETE) and in this blog post used for managing employee data. 

The main advantage of this is that you can package your Jarkata REST application into a JAR file which can be executed almost anywhere.


First, Create a Java with Maven > Java Application with Netbeans and add the following dependency to the pom.xml. 
In addition to the Jakarta Core Profile, Dependencies to support JSON-B, JSON-P, HTTP Server, and Jersey + Weld integration is also added to the pom.xml file of the project and
is explained more in details here by Rudy - [Run your Jakarta Application without Runtime](https://www.atbash.be/2023/01/05/run-your-jakarta-application-without-runtime/ "Run your Jakarta Application without Runtime"){:target="_blank"}. 
. The project pom.xml file should look like this

```xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>omos.microsystems.coreprofile.app</groupId>
    <artifactId>coreprofile-app</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <jersey.version>3.1.0</jersey.version>
        <exec.mainClass>omos.microsystems.coreprofile.app.CoreprofileApp</exec.mainClass>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-core-api</artifactId>
            <version>10.0.0</version>
            <scope>provided</scope>
        </dependency>       
        <dependency>
            <groupId>org.glassfish.jersey.inject</groupId>
            <artifactId>jersey-cdi2-se</artifactId>
            <version>${jersey.version}</version>
        </dependency>
        <dependency>
            <groupId>org.glassfish.jersey.media</groupId>
            <artifactId>jersey-media-json-binding</artifactId>
            <version>${jersey.version}</version>
        </dependency>
        <dependency>
            <groupId>org.glassfish.jersey.media</groupId>
            <artifactId>jersey-media-json-processing</artifactId>
            <version>${jersey.version}</version>
        </dependency>
        <dependency>
            <groupId>org.glassfish.jersey.containers</groupId>
            <artifactId>jersey-container-jdk-http</artifactId>
            <version>${jersey.version}</version>
        </dependency>    
        <dependency>
            <groupId>jakarta.activation</groupId>
            <artifactId>jakarta.activation-api</artifactId>
            <version>2.0.1</version>
        </dependency>     
    </dependencies>
    
     <profiles>
        <profile>
            <!-- here mainly to know the total size of the app.-->
            <id>exec</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-shade-plugin</artifactId>
                        <version>3.3.0</version>
                        <executions>
                            <execution>
                                <goals>
                                    <goal>shade</goal>
                                </goals>
                                <configuration>
                                    <shadedArtifactAttached>false</shadedArtifactAttached>
                                    <transformers>
                                        <transformer
                                            implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                            <mainClass>omos.microsystems.coreprofile.app.CoreprofileApp</mainClass>
                                        </transformer>
                                    </transformers>
                                </configuration>
                            </execution>
                        </executions>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>
    
</project>
```
The Jakarta Core Profile has the following api's - 

* jakarta.annotation-api-2.1.1
* jakarta.enterprise.cdi-api-4.0.1
* jakarta.enterprise.lang-model-4.0.1
* jakarta.inject-api-2.0.1
* jakarta.interceptor-api-2.0.1
* jakarta.json-api-2.1.0
* jakarta.json.bind-api-3.0.0
* jakarta.ws.rs-api-3.1.0


Next, Create the Java Class that extends the Application class in Jakarta REST. The Java Class for this is shown below

```java

package omos.microsystems.coreprofile.app;

import jakarta.ws.rs.ApplicationPath;
import jakarta.ws.rs.core.Application;

@ApplicationPath("")
public class JakartaRestConfiguration extends Application {
    
}
```

Next, Modify the main method of the Java class of the application to start the JAX-RS/Jakarta REST server within the code. The code to handle that
is shown below

```java
package omos.microsystems.coreprofile.app;

import jakarta.ws.rs.SeBootstrap;
import java.util.concurrent.CountDownLatch;
import org.glassfish.jersey.server.ResourceConfig;

public class CoreprofileApp {

    
    public static void main(String[] args) throws Exception {

        SeBootstrap.Configuration.Builder configBuilder = SeBootstrap.Configuration.builder();
        configBuilder.property(SeBootstrap.Configuration.PROTOCOL, "HTTP")
                .property(SeBootstrap.Configuration.HOST, "localhost")
                .property(SeBootstrap.Configuration.PORT, 8080);

        ResourceConfig resourceConfig = new ResourceConfig();
        resourceConfig.packages(JakartaRestConfiguration.class.getPackageName());
        SeBootstrap.start(resourceConfig, configBuilder.build());
        
        keepRunning();
    }
    
         private static void keepRunning() {
        CountDownLatch latch = new CountDownLatch(1);
        try {
            latch.await();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}


```

Next Create a META-INF folder inside the src/main/resources directory, and add an empty beans.xml file to it. The beans.xml file should look like this

```xml
<beans/>
```

Next, Create a Java Class named Employee.java to be used as the domain model for the application. The Restful web services that is about to be created would allow 
clients to perform CRUD operations on this Employee class. The Employee.java class is shown below

```java
package omos.microsystems.coreprofile.app;

import java.util.Objects;

public class Employee {
    
    private int id;
    private String firstname;
    private String lastname;
    private String jobTitle;

    public Employee() {
    }

    public Employee(int id) {
        this.id = id;
    }

    public Employee(int id, String firstname, String lastname, String jobTitle) {
        this.id = id;
        this.firstname = firstname;
        this.lastname = lastname;
        this.jobTitle = jobTitle;
    }

    // Note that getter , setter, hashcode and equals method are removed for brevity

    @Override
    public String toString() {
        return "Employee{" + "id=" + id + ", firstname=" + firstname + ", lastname=" + lastname + ", jobTitle=" + jobTitle + '}';
    }
    
}

```

Next, Create an EmployeeService.java class that will be used for our DAO (Data Access Object) to manage CRUD operations on the Employee object. In this project, A list is used
for the database/datasource to store the Employee data and keep things simple as no real database is used. The EmployeeService.java class looks like this:

```java
package omos.microsystems.coreprofile.app;

import jakarta.enterprise.context.ApplicationScoped;
import java.util.ArrayList;
import java.util.List;


@ApplicationScoped
public class EmployeeService {

    static List<Employee> employeeList = new ArrayList();

    static {
        employeeList.add(new Employee(1, "Omos", "Aziegbe", "Sales"));
        employeeList.add(new Employee(2, "Bill", "Withers", "Accountant"));
        employeeList.add(new Employee(3, "Marcus", "Garvey", "Software Developer"));

    }

    public List<Employee> getAllEmployees() {
        return employeeList;
    }

    public int addEmployee(Employee employee) {
        int newId = employeeList.size() + 1;
        employee.setId(newId);
        employeeList.add(employee);
        return newId;
    }

    public Employee getEmployee(int id) {
        Employee empToFind = new Employee(id);
        int index = employeeList.indexOf(empToFind);
        if (index >= 0) {
            return employeeList.get(index);
        }
        return null;
    }

    public void delete(Employee employee) {
        if (!employeeList.contains(employee)) { 
           employeeList.add(employee);
        }
        employeeList.remove(employee);
    }

//    public int updateEmployee(int empId, Employee emp) {
//    employeeList.set(empId, emp);
//    return emp.getId();
//}

    public boolean update(Employee employee) {
        int index = employeeList.indexOf(employee);
        if (index >= 0) {
            employeeList.set(index, employee);
            return true;
        }
        return false;
    }

    public static List<Employee> getEmployeeList() {
        return employeeList;
    }

    public Employee findById(int index) {
        return employeeList.get(index);
    }
}

```

Next, Create an EmployeeResource.java class that provides the REST API endpoints/URIs for the application. The EmployeeResource.java
class should look like this

```java

package omos.microsystems.coreprofile.app;

import jakarta.enterprise.context.ApplicationScoped;
import jakarta.inject.Inject;
import jakarta.ws.rs.Consumes;
import jakarta.ws.rs.DELETE;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.POST;
import jakarta.ws.rs.PUT;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.PathParam;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;
import jakarta.ws.rs.core.Response;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.Optional;


@Path("/employees")
@ApplicationScoped
public class EmployeeResource {

    @Inject
    EmployeeService employeeService;

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    @Consumes(MediaType.APPLICATION_JSON)
    public Response getAll() {
        return Response.ok(employeeService.getAllEmployees()).build();
    }

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    public Response add(Employee employee) throws URISyntaxException {
        int newEmployeeId = employeeService.addEmployee(employee);
        URI uri = new URI("/employees/" + newEmployeeId);
        return Response.created(uri).build();
    }

    @PUT
    @Consumes(MediaType.APPLICATION_JSON)
    @Path("{id}")
    public Response update(@PathParam("id") int id, Employee employee) {
        Employee updateEmployee = employeeService.findById(id);
        updateEmployee.setFirstname(employee.getFirstname());
        updateEmployee.setLastname(employee.getLastname());
        updateEmployee.setJobTitle(employee.getJobTitle());
        employeeService.update(updateEmployee);
        return Response.ok().build();
    }

    @GET
    @Path("{id}")
    @Produces(MediaType.TEXT_PLAIN)
    public String getEmployee(@PathParam("id") int id) {
        Optional<Employee> match = employeeService.getAllEmployees().stream().filter(c -> c.getId() == id).findFirst();
        if (match.isPresent()) {
            return "   ----------------    Employee Details:    ----------------   \n\n" + match.get().toString();
        } else {
            return "Employee not found";
        }
    }

    @DELETE
    @Path("{id}")
    public Response delete(@PathParam("id") int id) {
        Employee getEmployee = employeeService.findById(id);
        employeeService.delete(getEmployee);
        return Response.ok().build();
    }
}

```
In the EmployeeResource.java class shown above, we use the

@Path annotation define in the class to specify the URI path to which the resource responds.

We use the @Inject to inject the EmployeeService. 

We annotate the getAll() method with the @GET annotation which corresponds to the HTTP GET Request method 
which returns a list of Employee in JSON format. 

We annotate the add(Employee employee) method with the @POST annotation which corresponds to the HTTP POST Request method and 
is used to add a new Employee of JSON content type.

We annotate the update(@PathParam("id") int id, Employee employee) method with the @PUT annotation which corresponds to the HTTP PUT Request method and takes the ID of the 
Employee which is passed as a path parameter to the Request URI using the @PathParam annotation and updates an existing item with the ID passed to it as a path parameter passed to it in the Request URI.

We annotate the getEmployee(@PathParam("id") int id) method with the @GET annotation and also annotate this method with @Path("{id}") which returns information
of a specific employee based on the supplied ID given in the URI and returns a plain text format using the @Produces(MediaType.TEXT_PLAIN) annotation.

We annotate the delete(@PathParam("id") int id) method with the @DELETE annotation which corresponds to HTTP DELETE method and deletes an Employee object
that matches the ID of the Employee passed to it as a path parameter in the Request URI.
 