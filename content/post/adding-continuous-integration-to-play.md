+++
author = "Ricky Yim"
categories = ["Play!", "Play", "Continuous Integration", "Testing", "Travis"]
date = 2013-12-20T01:00:00Z
description = ""
draft = false
slug = "adding-continuous-integration-to-play"
tags = ["Play!", "Play", "Continuous Integration", "Testing", "Travis"]
title = "Adding Continuous Integration to Play!"
aliases = [
    "/adding-continuous-integration-to-play/"
]
+++

<p>With services like <a href="http://travis-ci.org">Travis</a>, it has become quite easy to add Continuous Integration to projects. This post will show you quickly how to setup Travis to build and test your <a href="http://www.playframework.org">Play</a> project. This assumes you have signed up for Github and Travis, and have authorised travis to connect to your github account. If you have not done this already, follow the Travis steps <a href="http://about.travis-ci.org/docs/user/getting-started/">here</a>.
</p>


<ol>
<li>Create a .travis.yml file in the root of your Play project.</li>
<li><p>Add the following contents.</p>
<pre class="prettyprint linenums prettyprinted">language: java
env:
  - PLAY_VERSION=2.2.1
before_script:
  - wget http://downloads.typesafe.com/play/${PLAY_VERSION}/play-${PLAY_VERSION}.zip
  - unzip -q play-${PLAY_VERSION}.zip
script: play-${PLAY_VERSION}/play test
</pre>
<li>

<p>Ensure travis is building your repository. Click on <a href="https://travis-ci.org/profile">https://travis-ci.org/profile</a> and ensure that your github repository is synchronised and building. Upon committing, your travis build should start.<p>

<p> To save SBT from redownloading dependencies, you can use the experimental caching feature of Travis. However this is only available for private repositories. </p> 

<pre class="prettyprint linenums prettyprinted">language: java
cache:
  directories:
  - .sbt
env:
  - PLAY_VERSION=2.2.1
before_script:
  - wget http://downloads.typesafe.com/play/${PLAY_VERSION}/play-${PLAY_VERSION}.zip
  - unzip -q play-${PLAY_VERSION}.zip
script: play-${PLAY_VERSION}/play test
</pre>
</li>
<li><p>The source to this post can be found <a href="https://github.com/codingricky/sample-play-with-travis">here</a></li>
</ol>

<p></p>