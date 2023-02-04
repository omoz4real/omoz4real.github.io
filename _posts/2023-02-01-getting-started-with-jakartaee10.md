---
layout: post
title: "Getting Started with Jakarta EE 10 in Netbeans "
date: 2023-02-01 
---

Jakarta EE 10 was releasesd with three different profiles - **Full platform profile**, **Web Profile** and **Core Profile**. To get started with Jakarta EE 10 in Netbeans, 
Create Maven project by going to File > New Project then choose Java with Maven > Web Application and complete the wizard. Next include the following dependency in the pom.xml

To use the Full Platform profile in the project. Add the following dependency to the pom.xml file

```java

<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-api</artifactId>
    <version>10.0.0</version>
    <scope>provided</scope>
</dependency>

```

To use the Web profile in the project. Add the following dependency to the pom.xml file

```java

<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-web-api</artifactId>
    <version>10.0.0</version>
    <scope>provided</scope>
</dependency>

```

To use the Core profile in the project. Add the following dependency to the pom.xml file

```java

<dependency>
  <groupId>jakarta.platform</groupId>
  <artifactId>jakarta.jakartaee-core-api</artifactId>
  <version>10.0.0</version>
  <scope>provided</scope>
</dependency>

```






