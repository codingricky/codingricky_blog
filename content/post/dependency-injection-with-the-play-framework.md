+++
author = "Ricky Yim"
categories = ["Play", "Guice", "Dependency Injection"]
date = 2014-12-08T04:18:25Z
description = ""
draft = false
slug = "dependency-injection-with-the-play-framework"
tags = ["Play", "Guice", "Dependency Injection"]
title = "Play-ing with Dependency Injection"
aliases = [
    "/dependency-injection-with-the-play-framework/"
]
+++

A few months ago I wrote about the [lack of Dependency Injection in Play](http://codingricky.com/guicing-up-the-play-framework-dependency-injection-with-guice-and-play/). As part of the 2.4 release of Play, Dependency Injection has been incorporated as part of the framework. [JSR 330](https://jcp.org/aboutJava/communityprocess/final/jsr330/) is now implemented using [Guice](https://github.com/google/guice). This article will demonstrate how to use it. As a reference point, I will be using the same example as the one used in [my previous article](http://codingricky.com/guicing-up-the-play-framework-dependency-injection-with-guice-and-play/).

Note that this article uses a non final release of the [Play Framework 2.4 (2.4.0-M2)](https://github.com/playframework/playframework/releases/tag/2.4.0-M2) so part of the framework may change before the final release.

Assuming you have the Play Framework already installed and you have created a sample application. If you require help, please refer to the instructions [here](https://www.playframework.com/documentation/2.4.x/NewApplication).

1.Create a simple service that we will be injecting into our controller. First create an interface at the path **app/services/GreetingService.java**

```java
package services;

public interface GreetingService {  
    String greeting();
}
```

Followed up by its implementation **app/services/RealGreetingService.java**.

```java
package services;

public class RealGreetingService implements GreetingService {  
    @Override
    public String greeting() {
        return "bonjour";
    }
}
```

2.Back in the **GreetingService** interface, add the **@ImplementedBy** Guice annotation to it.

```java
@ImplementedBy(RealGreetingService.class)
public interface GreetingService {
  public String greeting();
}
```

3.Go to the Application controller and inject the **GreetingService** into it. The key things here are the instance variable with the **@Inject** annotation and the controller index method not being static anymore, as it needs access to the **greetingService** instance variable.

```java
public class Application extends Controller {
  private final GreetingService greetingService;

  @Inject
  public Application(GreetingService greetingService) {
    this.greetingService = greetingService;
  }

  public Result index() {
    return ok(index.render(greetingService.greeting()));
  }
}
```

4.Start the application and go to the website at http://localhost:9000. It should look like this.

![](/images/2014/12/bonjour-1.jpg)

I've upload the sample application here on https://github.com/codingricky/play-di-java. As you can see, it is much simplier now that DI has been incorporated directly into the framework. 

###References###
* [Guicing up the Play Framework - Dependency Injection with Guice and Play](http://codingricky.com/guicing-up-the-play-framework-dependency-injection-with-guice-and-play/)
* [JSR 330](https://jcp.org/aboutJava/communityprocess/final/jsr330/)
* [Google Guice](https://github.com/google/guice)
* [Dependency injection with Play](https://www.playframework.com/documentation/2.4.0-M2/JavaDependencyInjection)
