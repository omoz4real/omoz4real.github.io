---
layout: post
title: "Persist List of String with JPA  "
date: 2023-01-20 
---

Sometimes you might want to store a List of string as one column field in your database. The following code snippet 
helped me achieve this. First define the Entity Class as shown below

```java
public class Customer implements Serializable {
    
    private static final long serialVersionUID = 1L;
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Basic(optional = false)
    @Column(name = "id", nullable = false)
    private Integer id;
    @Basic(optional = false)
    @NotNull
    @Size(min = 1, max = 245)
    @Column(name = "firstname", length = 245)
    private String firstname;
    
    @Basic(optional = true)
    @Column(name = "middlename", length = 245)
    private String middlename;
    
    @Basic(optional = true)
    @NotNull
    @Size(min = 1, max = 245)
    @Column(name = "lastname", length = 245)
    private String lastname;
    
    @Basic(optional = true)
    @NotNull
    @Size(min = 1, max = 245)
    @Column(name = "primary_email", length = 245)
    private String primaryEmail;
    
    @Size(max = 245)
    @Column(name = "previousCourse", length = 245)
    List<String> previousCourse;

```

Next, Add a converter to convert the database column to a list of string in the java Object and like wise the Java object will be stored in the database column
as Physics;Biology;Chemistry. The following code below shows the Converter class

```java
import java.util.Arrays;
import java.util.List;

import javax.persistence.AttributeConverter;
import javax.persistence.Converter;

import static java.util.Collections.*;


@Converter
public class StringListConverter implements AttributeConverter<List<String>, String> {

    private static final String SPLIT_CHAR = ";";

    @Override
    public String convertToDatabaseColumn(List<String> stringList) {
        return stringList != null ? String.join(SPLIT_CHAR, stringList) : "";
    }

    @Override
    public List<String> convertToEntityAttribute(String string) {
        return string != null ? Arrays.asList(string.split(SPLIT_CHAR)) : emptyList();
    }
}

```
Finally, use the Converter in the entity field like this

```java
    @Column(name = "previousCourse", length = 245)
    @Convert(converter = StringListConverter.class)
    List<String> previousCourse;

```
Another way to do this is to use the @ElementCollection in JPA.
