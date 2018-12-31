+++
author = "Ricky Yim"
categories = ["webjars", "Play!", "Play"]
date = 2013-11-01T01:00:00Z
description = ""
draft = false
slug = "managing-web-dependencies-in-play-with-webjars-2"
tags = ["webjars", "Play!", "Play"]
title = "Managing Web Dependencies in Play with WebJars"
aliases = [
    "/managing-web-dependencies-in-play-with-webjars-2/"
]
+++

<p>A common need in applications is to manage your web dependencies like <a href="http://getbootstrap.com">Bootstrap</a> and <a href="http://jquery.com">jQuery</a>. The simplest way of doing this is to download the appropriate files, store them in your application directory and reference them in your application. There are a couple of disadvantages to this approach. </p>

<p>For starters, it is not always clear which version of the asset was downloaded. Sometimes it is encoded in the name of the file or the directory structure, for e.g., "jquery-1.9.0.min.js", but I guess the onus is on the development team to keep it this way. The other disadvantage I see with this approach is it is quite manual. For example, updating versions of javascript libraries with their respective dependencies can be rather tedious. So enter <a href="http://www.webjars.org/">WebJars</a>, a very simple library that integrates with <a href="http://www.playframework.com">Play</a> as well as other web frameworks, that packages client-side web libraries as JAR files and allows for them to be managed as SBT dependencies (or Maven and Gradle). This article will show you the steps required to integrate WebJars into a sample Play application.</p><br />
<ol>
<li>
The first thing you need to do is edit the <b>build.sbt</b> file. Add the WebJar dependency. Your <b>build.sbt</b> file should look like the following.<br />


<pre class="prettyprint linenums">name := "sample-play-with-webjars"

version := "1.0-SNAPSHOT"

libraryDependencies ++= Seq(
  javaJdbc,
  javaEbean,
  cache,
  "org.webjars" %% "webjars-play" % "2.2.0"
)     

play.Project.playJavaSettings
</pre>
</li>
<li>
Add a WebJars dependency like <a href="http://getboostrap.com">Bootstrap</a> to the <b>build.sbt</b> file. You can find a list of dependencies on the official WebJars <a href="http://www.webjar.org/">site</a>. I'm including Bootstrap into my Play application as an example.<br />


<pre class="prettyprint linenums">name := "sample-play-with-webjars"

version := "1.0-SNAPSHOT"

libraryDependencies ++= Seq(
  javaJdbc,
  javaEbean,
  cache,
  "org.webjars" %% "webjars-play" % "2.2.0",
  "org.webjars" % "bootstrap" % "3.0.1"
)     

play.Project.playJavaSettings
</pre>
</li>
<li>
Now go to your <b>conf/routes</b> file and add in a route to the <b>WebJarsAssets</b> controller that will be used when accessing the dependencies.<br />


<pre class="prettyprint linenums"># Routes
# This file defines all application routes (Higher priority routes first)
# ~~~~

# Home page
GET     /                           controllers.Application.index()

# Map static resources from the /public folder to the /assets URL path
GET     /assets/*file               controllers.Assets.at(path="/public", file)

GET     /webjars/*file                    controllers.WebJarAssets.at(file)
</pre>
</li>
<li>
Now add a reference to the bootstrap css from your <b>app/views/main.scala.html</b> file via the WebJar route we just defined.<br />


It should look like this:<br />

<pre class="prettyprint linenums">@(title: String)(content: Html)

&lt;!DOCTYPE html&gt;

&lt;html&gt;
    &lt;head&gt;
        &lt;title&gt;@title&lt;/title&gt;
        &lt;link rel="stylesheet" media="screen" href="@routes.Assets.at("stylesheets/main.css")"&gt;
        &lt;link rel="shortcut icon" type="image/png" href="@routes.Assets.at("images/favicon.png")"&gt;
        &lt;link rel='stylesheet' href='@routes.WebJarAssets.at(WebJarAssets.locate("css/bootstrap.min.css"))'&gt;
        &lt;script src="@routes.Assets.at("javascripts/jquery-1.9.0.min.js")" type="text/javascript"&gt;&lt;/script&gt;
    &lt;/head&gt;
    &lt;body&gt;
        @content
    &lt;/body&gt;
&lt;/html&gt;


</pre>
</li>
<li>
To prove that everything works, I've changed <b>app/views/index.scala.html</b> to use the sample progress bar from Bootstrap to make it really obvious that WebJars has been configured properly and that the Bootstrap dependency has been resolved. Your file should look like the following:<br />


<pre class="prettyprint linenums">@(message: String)

@main("Testing out WebJars") {


      &lt;div class="page-header"&gt;
        &lt;h1&gt;Progress bars&lt;/h1&gt;
      &lt;/div&gt;
      &lt;div class="progress"&gt;
        &lt;div class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;"&gt;&lt;span class="sr-only"&gt;60% Complete&lt;/span&gt;&lt;/div&gt;
      &lt;/div&gt;

}
</pre>
<div>
<br /></div>
<pre></pre>
</li>
<li>
Now if you start up the application and navigate to http://localhost:9000, your default home page should look like the following.<br />


<pre class="prettyprint linenums">$ play run
</pre>

<a href="http://1.bp.blogspot.com/-7lYj96PEwFs/UnOLhgajb_I/AAAAAAAAC7k/1bGG3rluTzU/s1600/sample+webjars.png" imageanchor="1" ><img border="0" src="http://1.bp.blogspot.com/-7lYj96PEwFs/UnOLhgajb_I/AAAAAAAAC7k/1bGG3rluTzU/s640/sample+webjars.png" /></a>

</li>
</ol>
The sample code for this article is up <a href="https://github.com/codingricky/sample-play-with-webjars">here</a>. I hope this article showed you a better way of managing your web dependencies.