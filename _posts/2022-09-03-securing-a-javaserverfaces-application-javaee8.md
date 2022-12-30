---
layout: post
title: "Securing a JavaServer Faces Application - Java/Jakarta EE 8 "
date: 2022-09-09 
---
This blog post is about securing a Java Server Faces application that can be deployed to a Java/Jakarta EE 8 compliant
application server. The web application uses a custom JSF form login page to access secure directories. This post also shows how you can
authenticate users against an H2 database to secure the JSF application.

The following software and resources is used in developing this application

* Netbeans IDE 13
* JDK 11
* Java EE 8
* PrimeFaces Version 10
* H2 Database
* Java/Jakarta EE 8 compliant application server


First, Create the web application project in Netbeans and choose the Java/Jakarta EE version and application server of your choice. In this instance I have the Payara Server Community Version and Jakarta EE 8 selected.

Next update the pom.xml file of the project to add dependecies for H2 database and Primefaces. The pom.xml should look like the code shown below

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>omos.microsystems</groupId>
    <artifactId>jsfAuthentication</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>

    <name>jsfAuthentication</name>

    <properties>
        <endorsed.dir>${project.build.directory}/endorsed</endorsed.dir>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <jakartaee>8.0.0</jakartaee>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-api</artifactId>
            <version>${jakartaee}</version>
            <scope>provided</scope>
        </dependency>
        
        <dependency>
            <groupId>org.primefaces</groupId>
            <artifactId>primefaces</artifactId>
            <version>10.0.0</version>
        </dependency>
        
        <dependency>
            <groupId>org.primefaces.themes</groupId>
            <artifactId>all-themes</artifactId>
            <version>1.0.10</version>
        </dependency>
       
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <version>2.1.210</version>
            <scope>runtime</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <compilerArguments>
                        <endorseddirs>${endorsed.dir}</endorseddirs>
                    </compilerArguments>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                </configuration>
            </plugin>

        </plugins>
    </build>

    <repositories>
        <repository>
            <id>prime-repo</id>
            <name>PrimeFaces Maven Repository</name>
            <url>https://repository.primefaces.org</url>
            <layout>default</layout>
        </repository>
    </repositories>
</project>
```


Next we configure the Datasource for the application by adding the following code to the **web.xml** file of the project. The web.xml file is located
in the **WEB-INF** directory which is located inside the **src/main/webapp** of the project.


```xml
    <data-source>
        <name>java:global/JSFAuthentication</name>
        <class-name>org.h2.jdbcx.JdbcDataSource</class-name>
        <url>jdbc:h2:file:/Users/omozegieaziegbe/Development/databases/SecurityDB;DB_CLOSE_ON_EXIT=FALSE</url>
    </data-source>
```


This configuration will create a H2 file based database called SecurityDB in the location specified in the URL.
I also added a servlet name and mapping for the H2 database so as to be able to view and edit the contents of the database from a web browser. The servlet name and mapping added to the web.xml is shown below


```
    <servlet>
        <servlet-name>h2-console</servlet-name>
        <servlet-class>org.h2.server.web.WebServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>h2-console</servlet-name>
        <url-pattern>/h2/*</url-pattern>
    </servlet-mapping>
```


Next we create a **persistence.xml** located in the **META-INF** directory which is located inside the **src/main/resources** folder of the root project.
The persistence.xml is the main configuration file for JPA in Java/Jakarta EE applications that defines the name of the persistence unit and the data source. The content of the
persistence.xml looks like this


```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.1" xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd"> 
    <persistence-unit name="FacesAuthenticationPU" transaction-type="JTA">
        <jta-data-source>java:global/JSFAuthentication</jta-data-source>
        <properties>
            <property name="javax.persistence.schema-generation.database.action" value="create" />
        </properties>
    </persistence-unit>
</persistence>
```


Next, we create the directories/folders that we will secure in our application. This can be done within the Netbeans IDE by right-clicking the Web Pages folder
and choosing **New > Other > Folder** and click on the next button to enter a name for the folder. In this application I created two folders
named **admin** and **user** to represent the secured admin folder and secured user folder respectively. 

The pages to be secured are also created in each of the folders. Therefore we need to create a JSF page in the admin folder and a JSF page
in the user folder. 
To do this right-click on the admin folder, choose New > JSF page with name index.xhtml and the same for the user folder
to add the secured pages.








[Eapps Jelastic Platform as a Service](https://portal.eapps.com/aff.php?aff=2289 "Eapps Jelastic PaaS"){:target="_blank"}
