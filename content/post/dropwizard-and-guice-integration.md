+++
author = "Ricky Yim"
date = 2014-08-04T01:42:11Z
description = ""
draft = false
slug = "dropwizard-and-guice-integration"
title = "Dropwizard and Guice integration"
aliases = [
    "dropwizard-and-guice-integration/"
]
+++

[Dropwizard](https://dropwizard.github.io/dropwizard/), as described on their website is a *"Java framework for developing ops-friendly, high-performance, RESTful web services."* I have been using it recently after seeing it mentioned on my company's Yammer feed (funnily enough the framework emerged itself from Yammer) and have been enjoying it very much. One of the things I integrated into my setup of Dropwizard was [Guice](https://github.com/google/guice), as quite quickly I found I needed a way to inject my dependencies. The rest of this article will be devoted into showing you how to glue Dropwizard and Guice together.

![](/content/images/2014/Aug/dropwizard-logo.png)

###Prerequisites

* Working JDK (I'm using Java 1.8)
* Maven 3


### Getting started

Start by using the Maven quickstart archetype to generate the shell of a Maven project. So in a terminal, perform the following command. Feel free to change the groupId and artifactId to suit your own purposes. 

```shell
$ mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.github.codingricky -DartifactId=DropWizardGuice -Dversion=1.0-SNAPSHOT
```


Now update the dependencies in the **pom.xml** to include what is required for this article.

```xml

    <dependencies>
        <dependency>
            <groupId>io.dropwizard</groupId>
            <artifactId>dropwizard-core</artifactId>
            <version>0.7.0</version>
        </dependency>
        <dependency>
            <groupId>com.hubspot.dropwizard</groupId>
            <artifactId>dropwizard-guice</artifactId>
            <version>0.7.0.2</version>
        </dependency>
        <dependency>
            <groupId>com.google.inject</groupId>
            <artifactId>guice</artifactId>
            <version>3.0</version>
        </dependency>
    </dependencies>
```

This adds in the **dropwizard** framework, the **dropwizard guice** extension and **guice** itself.

###Adding configuration

What this demo will do, is take a message from the **configuration yml file** and inject it as a dependency into a **resource**. 

Let's now add a configuration class. This will be a **dropwizard** configuration class so it will need to extend the **io.dropwizard.Configuration** class. Add a **message** field with corresponding getters/setters so dropwizard can get the message to inject into our resource.

```java
package com.github.codingricky;

import io.dropwizard.Configuration;
import org.hibernate.validator.constraints.NotEmpty;

public class ServerConfiguration extends Configuration {
    @NotEmpty
    private String message;
    public String getMessage() {
        return message;
    }
}
```
Now create a **config.yml** and add this message property in it. Put this in the root directory of the project, so at the same level as the **pom.xml** file.

```yml
message: "Bonjour"
```

###Adding the resource

The **resource** in dropwizard speak represents the REST resource. It is the handler of the http request. We will create one that responds to the path **/hello** and it will output our message from the **config.yml** file. This is where **Guice** will get involved. Create this class in **src/main/java**.

```java
package com.github.codingricky;

import com.google.inject.Inject;
import com.google.inject.name.Named;

import javax.ws.rs.GET;
import javax.ws.rs.Path;

@Path("/hello")
public class HelloResource {
    private final String message;

    @Inject
    public HelloResource(@Named("message") String message) {
        this.message = message;
    }

    @GET
    public String hello() {
        return message;
    }
}
```

This class now expects Guice to inject a **@Named** property named **message**.

### Now the Guice module

Now we need a bit of glue code that takes the message from the configuration file to make it available to **Guice**. Create a **ServerModule** that implements the **com.google.inject.Module**. 

```java
package com.github.codingricky;

import com.google.inject.Binder;
import com.google.inject.Module;
import com.google.inject.Provides;

import javax.inject.Named;

public class ServerModule implements Module {
    @Override
    public void configure(Binder binder) {
    }

    @Provides
    @Named("message")
    public String provideMessage(ServerConfiguration serverConfiguration) {
        return serverConfiguration.getMessage();
    }
}
```

Take special note that of the **provideMessage** that extracts the message from the **ServerConfiguration**.

### Finally the application

Now we need to tie everything together. The application class will start the application and also will create a **Guice bundle** that encapsules the **ServerModule** and the **ServerConfiguration**. 

```java
package com.github.codingricky;

import com.hubspot.dropwizard.guice.GuiceBundle;
import io.dropwizard.Application;
import io.dropwizard.setup.Bootstrap;
import io.dropwizard.setup.Environment;

public class ServerApplication extends Application<ServerConfiguration> {
    @Override
    public void initialize(Bootstrap<ServerConfiguration> bootstrap) {
        GuiceBundle<ServerConfiguration> guiceBundle = GuiceBundle.<ServerConfiguration>newBuilder()
                .addModule(new ServerModule())
                .setConfigClass(ServerConfiguration.class)
                .enableAutoConfig(getClass().getPackage().getName())
                .build();
        bootstrap.addBundle(guiceBundle);
    }

    @Override
    public void run(ServerConfiguration configuration, Environment environment) throws Exception {

    }

    public static void main(String[] args) throws Exception {
        new ServerApplication().run(args);
    }
}
```

### Let's Go!

The final step is to package everything up and run it.

So add in the plugin details that will package up the application and allow it to be run via the commandline.

```xml
<plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>1.6</version>
                <configuration>
                    <createDependencyReducedPom>true</createDependencyReducedPom>
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>com.github.codingricky.ServerApplication</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
```

Now we can build everything via Maven.

```shell
$ mvn clean install
```

Finally we can start the application

```shell
$ java -jar target/DropWizardGuice-1.0-SNAPSHOT.jar server config.yml
```

If the application starts up correctly, you should be able to go to a browser and hit the url, **http://localhost:8080/hello**

![](/content/images/2014/Aug/message.png)

### Summary
Thanks for reading, I hope this article was useful to you. Here are some references that were used in this article.

* https://dropwizard.github.io/dropwizard/
* https://github.com/HubSpot/dropwizard-guice
