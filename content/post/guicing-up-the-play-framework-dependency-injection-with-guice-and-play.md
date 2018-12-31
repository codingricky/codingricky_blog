+++
author = "Ricky Yim"
categories = ["Play!", "Play", "Guice", "Dependency Injection", "Modules"]
date = 2013-09-24T02:00:00Z
description = ""
draft = false
slug = "guicing-up-the-play-framework-dependency-injection-with-guice-and-play"
tags = ["Play!", "Play", "Guice", "Dependency Injection", "Modules"]
title = "Guicing up the Play Framework - Dependency Injection with Guice and Play"
aliases = [
    "/guicing-up-the-play-framework-dependency-injection-with-guice-and-play/"
]
+++

One of the features lacking out of the box for Play is Dependency Injection. Coming from a background of JEE, this seemed to be quite a significant omission. 

Fortunately it is not very difficult to set it up. This article will take you through the steps of adding Guice to your Play application.

<ol>
<li>The first step is to add the Guice dependency to your <b>build.sbt</b> file or <b>Build.scala</b> if you're using Play 2.1.X or lower.

Your <b>build.sbt</b> should look like this

<pre class="prettyprint linenums">
name := "sample-play-with-guice"

version := "1.0-SNAPSHOT"

libraryDependencies ++= Seq(
  javaJdbc,
  javaEbean,
  cache,
  "com.google.inject" % "guice" % "4.0-beta"
)     

play.Project.playJavaSettings
</pre>
</li>
<li>
Now let's create a simple service that we will be injecting into our controller.


First create an interface at the path <b>app/services/GreetingService.java</b>

<pre class="prettyprint linenums">
package services;

public interface GreetingService {
    String greeting();
}
</pre> 

Followed up by its implementation here <b>app/services/RealGreetingService.java</b>

<pre class="prettyprint linenums">
package services;

public class RealGreetingService implements GreetingService {
    @Override
    public String greeting() {
        return "bonjour";
    }
}
</pre> 
</li>
<li>
Now let's go to the <b>Application</b> controller and inject the <b>GreetingService</b> into it.

The key things here are the instance variable with the <b>@Inject</b> annotation and the controller <b>index</b> method not being static anymore, as it needs access to the greetingService instance variable. 

<pre class="prettyprint linenums">
package controllers;

import com.google.inject.Inject;
import play.mvc.Controller;
import play.mvc.Result;
import services.GreetingService;
import views.html.index.*;

public class Application extends Controller {

    @Inject
    private GreetingService greetingService;

    public Result index() {
        return ok(index.render(greetingService.greeting()));
    }

}

</pre> 

</li>
<li>
Go to your routes file and put an <b>@</b> in front of the index route to indicate that it is no longer static.

<pre class="prettyprint linenums">
# Routes
# This file defines all application routes (Higher priority routes first)
# ~~~~

# Home page
GET     /                           @controllers.Application.index()

# Map static resources from the /public folder to the /assets URL path
GET     /assets/*file               controllers.Assets.at(path="/public", file)

</pre> 
</li>
<li>
Lastly, create a Global.java class at <b>app/Global.java</b>.

Create an injector and override the <b>getControllerInstance</b> method to return instances from the injector. When a route has a prefix of <b>@</b>, Play will call this method.

<pre class="prettyprint linenums">
import com.google.inject.AbstractModule;
import com.google.inject.Guice;
import com.google.inject.Injector;
import play.Application;
import play.GlobalSettings;
import services.GreetingService;
import services.RealGreetingService;

public class Global extends GlobalSettings {

    private Injector injector;

    @Override
    public void onStart(Application application) {
        injector = Guice.createInjector(new AbstractModule() {
            @Override
            protected void configure() {
                bind(GreetingService.class).to(RealGreetingService.class);
            }
        });
    }

    @Override
    public &lt;T&gt; T getControllerInstance(Class&lt;T&gt; aClass) throws Exception {
        return injector.getInstance(aClass);
    }
}
</pre>
</li>
<li>
If this has all worked, you should be able to run the application and see the message by visiting http://localhost:9000.

<pre>
$ play run
</pre>

Your greeting should appear like the screenshot below.

<div class="separator" style="clear: both; text-align: center;"><a href="http://4.bp.blogspot.com/-29bLuNgM_vk/UkGBQs6kduI/AAAAAAAAAFM/qtBBJEIBqx0/s1600/screenshot.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://4.bp.blogspot.com/-29bLuNgM_vk/UkGBQs6kduI/AAAAAAAAAFM/qtBBJEIBqx0/s400/screenshot.png" /></a></div>
</li>

</ol> 


I've upload the sample application here on <a href="https://github.com/codingricky/sample-play-with-guice">https://github.com/codingricky/sample-play-with-guice</a>.