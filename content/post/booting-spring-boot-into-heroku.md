+++
author = "Ricky Yim"
categories = ["spring", "heroku", "spring boot", "deployment"]
date = 2014-01-17T07:19:25Z
description = ""
draft = false
slug = "booting-spring-boot-into-heroku"
tags = ["spring", "heroku", "spring boot", "deployment"]
title = "Booting Spring Boot into Heroku"
aliases = [
    "/booting-spring-boot-into-heroku/"
]
+++

[Spring Boot](http://projects.spring.io/spring-boot/) is a new approach from Spring to build web applications. The quote from Spring is that it *"takes an opinionated view of building production-ready Spring applications. Spring Boot favors convention over configuration and is designed to get you up and running as quickly as possible."* One of the things I like doing when evaluating a new framework is to ensure that I have an environment to deploy my work. So this post will show you how to get a sample Spring Boot application running on [heroku](http://www.heroku.com).

To get started, you need a few prerequistes. 

* JDK 1.7
* Maven 3
* A Herkou account
* Heroku command line tools
* Git
* An internet connection :)

The very first thing we need to do is create a very simple application that can be deployed. 

<ol>
<li> Firstly, we will create a vanilla maven project with the correct dependencies for Spring Boot setup. Create a **new directory (sample-spring-boot)** and a **pom.xml** file inside it. Put the following contents in the file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <!-- Inherit defaults from Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>0.5.0.M6</version>
    </parent>

    <!-- Add typical dependencies for a web application -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <!-- Package as an executable JAR -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <!-- Allow access to Spring milestones and snapshots -->
    <!-- (you don't need this if you are using anything after 0.5.0.RELEASE) -->
    <repositories>
        <repository>
            <id>spring-snapshots</id>
            <url>http://repo.spring.io/snapshot</url>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <url>http://repo.spring.io/milestone</url>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <url>http://repo.spring.io/snapshot</url>
        </pluginRepository>
        <pluginRepository>
            <id>spring-milestones</id>
            <url>http://repo.spring.io/milestone</url>
        </pluginRepository>
    </pluginRepositories>
</project>

```
</li>
<li> Create a sample controller under **src/main/java/SampleController.java**. It should have the following contents. 

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@Controller
@EnableAutoConfiguration
public class SampleController {

    @RequestMapping("/")
    @ResponseBody
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) throws Exception {
    	String webPort = System.getenv("PORT");
        if (webPort == null || webPort.isEmpty()) {
            webPort = "8080";
        }
        System.setProperty("server.port", webPort);
        SpringApplication.run(SampleController.class, args);
    }
}
```
</li>
<li> Let's start up our application to see if everything we have done to date is functional. Run the following command in a shell.

```shell
$ java -jar target/*.jar
```
</li>
<li> You should be able to now go to the http://localhost:8080 via your web browser or use curl to access it. 

```shell
$ curl http://localhost:8080
```
</li>
<li> Now let's create a Procfile so heorku will know how to start our application. So create a file with the name **Procfile** and put it in the root of your project. Add the following contents to the file.

```shell
web: java -jar target/*.jar
```
</li>
<li>Create a **system.properties** file that tells heroku to use Java 1.7. It should have the following contents.

```shell
java.runtime.version=1.7
```

</li>
<li>Add your project to a git repository. 

```shell
$ git init .
$ git add .
$ git commit -m 'initial add'
```
</li>
<li>Assuming you have the [heroku command line tools](https://devcenter.heroku.com/articles/heroku-command) installed, create a heroku application via the following command.

```shell
$ heroku create
```

</li>
<li>Now you should be able to deploy by pushing your code to the heroku remote branch.

```shell
$ git push heroku master
```
</li>
<li>If this has worked your application should now be deployed to heroku.

```shell
$ heroku open
```

</li>

I hope this article has shown you the steps required to deploy a Spring Boot application to herkou. The code for this application can be found [here](https://github.com/codingricky/sample-spring-boot).