---
layout: post
title: "Getting Started with Jakarta EE 10 in Netbeans "
date: 2023-02-01 
---


To get started with Jakarta EE 10 in Netbeans, Create Maven project by going to File > New Project then choose
Java with Maven > Web Application and complete the wizard. Next include the following dependency in the pom.xml
of the project.

```java

<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-api</artifactId>
    <version>10.0.0</version>
    <scope>provided</scope>
</dependency>

```

When this dependency is added, The Jakarta EE 10 platform full profile is available to be used in the project.






