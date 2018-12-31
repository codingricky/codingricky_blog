+++
author = "Ricky Yim"
categories = ["java", "testing", "faker"]
date = 2014-11-28T11:06:56Z
description = ""
draft = false
slug = "faking-it"
tags = ["java", "testing", "faker"]
title = "Faking Data"
aliases = [
    "/faking-it/"
]
+++

It's showcase time, your application has been deployed and the product owner comes around the corner. The login screen loads up but you realise there's no data in the system. Valuable seconds tick by and yet you can't think of any creative customer names besides from "John Smith" and "Jane Doe." 

Enter the [Java Faker library](http://dius.github.io/java-faker/). This is a port of the popular [Ruby Faker gem](https://github.com/stympy/faker). The idea behind the library is very simple. It generates fake data. The library actually came out of a software project developed by my company [DiUS Computing](http://www.dius.com.au). We used it to generate data for showcases. The rest of this article will show how this library is used.

 1.The first thing you need to do is add the dependency to your build tool. This example uses maven.

```xml
<dependency>
    <groupId>com.github.javafaker</groupId>
    <artifactId>javafaker</artifactId>
    <version>0.5</version>
</dependency>
```

 2.With the dependency resolved, you can begin to use the library.
 
 Start by instaniating an instance of the Faker.
 
```java
Faker faker = new Faker();
```
 
 3.Start creating fake data now. It is as simple as that.
 
```java
String name = faker.name().fullName();
String streetAddress = faker.address().streetAddress();
String emailAddress = faker.internet().emailAddress();
```
 Look at the [javadocs](http://dius.github.io/java-faker/apidocs/index.html) for examples of what fake data can be generated.
 
 I'm the current maintainer of the library, so if you have any ideas on how to make the library better or  any proposed changes, let me know or send me a pull request. Happy Faking!
 
 **References**
 
 * [Github page for Java Faker]( https://github.com/DiUS/java-faker)
 * [Javadocs](http://dius.github.io/java-faker/apidocs/index.html)
 * [Ruby faker](https://github.com/stympy/faker)

