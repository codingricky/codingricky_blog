+++
author = "Ricky Yim"
categories = ["java", "reflection", "reflections"]
date = 2014-05-02T07:08:43Z
description = ""
draft = false
slug = "reflections"
tags = ["java", "reflection", "reflections"]
title = "Reflection in Java made easy"
aliases = [
    "/reflections/"
]
+++

Reflection is one of the most powerful APIs available to a Java developer. Out of the box, the standard Java api is quite labourious to use, especially to search and query for particular methods. 

For example, on a project I was recently working, to retrieve all the **public** methods off a class that returned a **string** taking in **zero** parameters with a method naming starting with **to**, the code would have to look like this:

```java
 ArrayList<Method> results = new ArrayList<Method>();     
 for (Method m : String.class.getDeclaredMethods()) {                
     if (Modifier.isPublic(m.getModifiers()) &&           
             m.getReturnType().equals(String.class) &&    
             m.getParameterCount() == 0 &&                
             m.getName().startsWith("to")) {              
         results.add(m);                                  
     }                                                    
 }                                                                                                                  
```

So you could imagine, if you had anything more complicated, how this would end up looking. I looked around and found the [Reflections](https://github.com/ronmamo/reflections) library which makes this kind of work extremely easy. Converting the same query as above would look like this:

```java
  Set<Method> results = getMethods(String.class,
          withModifier(Modifier.PUBLIC),
          withReturnType(String.class),     
          withParametersCount(0),
          withPrefix("to"));                                              
```

There's a lot more complicated queries that could be achieved with the library. The [javadoc](http://reflections.googlecode.com/svn/trunk/reflections/javadoc/apidocs/index.html?org/reflections/Reflections.html) is a good place to look for more information. So in future, hopefully you consider using the library if you need to perform any reflections related operations in Java.

Here are some related links.

* https://github.com/ronmamo/reflections
* http://docs.oracle.com/javase/tutorial/reflect/