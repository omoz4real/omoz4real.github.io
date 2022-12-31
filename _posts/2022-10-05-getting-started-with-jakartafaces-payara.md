---
layout: post
title: "Getting Started with Jakarta Faces 4 on Payara Server "
date: 2022-10-05 
---

This blog post is about getting started with Jakarta Faces 4.0 on the Payara application server. Currently at the time of writing this blog post (05/10/2022), The version of payara server that is
compatible with Jakarta EE 10 and Faces 4.0 is Payara Server Community 6.2022.1 Alpha 4 which can be downloaded from the Payara Fish download
page located at [Payara platform community edition](https://www.payara.fish/downloads/payara-platform-community-edition/). 

The following software and resources are used in developing this project.

* Jakarta EE 10
* Netbeans IDE 15.0
* Payara Application Server - Community Version 6.2022.1

Firstly, download the server and unzip it to a location of your choice on your system. 

Next, create a Java maven web application with Netbeans with the following steps File > New Project > Java with Maven > Web Application.

Next, update the project pom.xml file to use the Jakarta EE 10 provided dependency. After updating the file, the pom.xml looks like this

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>omos.microsystems</groupId>
    <artifactId>securejakarta</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>
    <name>securejakarta</name>
    
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <endorsed.dir>${project.build.directory}/endorsed</endorsed.dir>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <failOnMissingWebXml>false</failOnMissingWebXml>
        <jakartaee>10.0.0</jakartaee>
    </properties>
    
    
    <dependencies>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-api</artifactId>
            <version>${jakartaee}</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    
    <build>
        <finalName>secure-jakarta-app</finalName>
    </build>
</project>
```

Next we configure the Datasource for the application by adding the following code to the **web.xml** file of the project. The web.xml file is located
in the **WEB-INF** directory which is located inside the **src/main/webapp** of the project.


```xml
    <data-source>
        <name>java:global/SecureFaces</name>
        <class-name>org.h2.jdbcx.JdbcDataSource</class-name>
        <url>jdbc:h2:file:/Users/omozegieaziegbe/Development/databases/SecureFacesDB;DB_CLOSE_ON_EXIT=FALSE</url>
    </data-source>
```

The code above would will configure/create a H2 file based database called SecureFacesDB in the location specified in the URL.
A servlet name and mapping for the H2 database is also added to the web.xml to view and edit the contents of the database from a web browser. After adding the servlet name and mapping configuration code to
file, the web.xml is file should look like the code shown below

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">
    <session-config>
        <session-timeout>
            30
        </session-timeout>
    </session-config>
    
     <welcome-file-list>
        <welcome-file>index.xhtml</welcome-file>
    </welcome-file-list>
    
    <servlet>
        <servlet-name>h2-console</servlet-name>
        <servlet-class>org.h2.server.web.WebServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>h2-console</servlet-name>
        <url-pattern>/h2/*</url-pattern>
    </servlet-mapping>
    
    <data-source>
        <name>java:global/SecureFaces</name>
        <class-name>org.h2.jdbcx.JdbcDataSource</class-name>
        <url>jdbc:h2:file:/Users/omozegieaziegbe/Development/databases/SecureFacesDB;DB_CLOSE_ON_EXIT=FALSE</url>
    </data-source>
</web-app>
```

Next add a faces-config.xml file to the project to use Faces 4.0. The faces-config.xml is added to the WEB-INF folder which is located inside the **src/main/webapp** of the project.
After updating the faces-config.xml file, the file should look like the code shown below

```xml
<?xml version='1.0' encoding='UTF-8'?>
<faces-config
    xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
        https://jakarta.ee/xml/ns/jakartaee/web-facesconfig_4_0.xsd"
    version="4.0">
</faces-config>
```

Next, create or update the **persistence.xml** file located in the **META-INF** directory which is located inside the **src/main/resources** folder of the root project.
The persistence.xml file is the main configuration file for persistence/JPA in Java/Jakarta EE applications which defines the name of the persistence unit and the data source. The content of the
persistence.xml is shown below

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="3.0" xmlns="https://jakarta.ee/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://jakarta.ee/xml/ns/persistence https://jakarta.ee/xml/ns/persistence/persistence_3_0.xsd">
    <persistence-unit name="FacesPU" transaction-type="JTA">
        <jta-data-source>java:global/SecureFaces</jta-data-source>
        <properties>
            <property name="jakarta.persistence.schema-generation.database.action" value="create"/>
<!--            <property name="jakarta.persistence.sql-load-script-source" value="insertEmployee.sql"/>-->
        </properties>
    </persistence-unit>
</persistence>
```

[Eapps Jelastic Platform as a Service](https://portal.eapps.com/aff.php?aff=2289 "Eapps Jelastic PaaS"){:target="_blank"}