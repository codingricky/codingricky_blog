+++
author = "Ricky Yim"
categories = ["protractor", "angularjs", "e2e", "testing"]
date = 2014-05-27T10:19:15Z
description = ""
draft = false
slug = "baby-steps-with-protractor-and-angularjs"
tags = ["protractor", "angularjs", "e2e", "testing"]
title = "Baby steps with Protractor and AngularJS"

+++

[Protractor](https://github.com/angular/protractor) is an emerging E2E (end to end) testing framework in the [AngularJS](https://angularjs.org/) world. This article shows you how to run your very first test in Protractor.

Before beginning, ensure you have the following pre-requisites installed.

* [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html?ssSourceSiteId=otnjp)
* [Node](http://nodejs.org/)
* [npm](https://www.npmjs.org/)
* [Chrome](https://www.google.com/intl/en/chrome/browser/)

The first thing you need to do is to install Protractor.

```
npm install -g protractor
```

As Protractor uses webdriver/selenium under the covers, you will need to ensure you have the necessary libaries installed.

```
webdriver-manager update
```

To get started, we'll just take the example configuration bundled with Protractor.

```
$ mkdir protractor
$ cd protractor
$ cp /usr/local/lib/node_modules/protractor/example/* .
```

Now in another terminal window, start up selenium server via the following command.

```
$ webdriver-manager start
```

You should see a bunch of output like:

```
seleniumProcess.pid: 7636
May 02, 2014 9:32:19 PM org.openqa.grid.selenium.GridLauncher main
INFO: Launching a standalone server
Setting system property webdriver.chrome.driver to /usr/local/lib/node_modules/protractor/selenium/chromedriver
21:32:19.465 INFO - Java: Oracle Corporation 23.7-b01
21:32:19.465 INFO - OS: Mac OS X 10.9.2 x86_64
21:32:19.477 INFO - v2.41.0, with Core v2.41.0. Built from revision 3192d8a
21:32:19.564 INFO - Default driver org.openqa.selenium.ie.InternetExplorerDriver registration is skipped: registration capabilities Capabilities [{platform=WINDOWS, ensureCleanSession=true, browserName=internet explorer, version=}] does not match with current platform: MAC
21:32:19.600 INFO - RemoteWebDriver instances should connect to: http://127.0.0.1:4444/wd/hub
21:32:19.601 INFO - Version Jetty/5.1.x
21:32:19.602 INFO - Started HttpContext[/selenium-server/driver,/selenium-server/driver]
21:32:19.603 INFO - Started HttpContext[/selenium-server,/selenium-server]
21:32:19.603 INFO - Started HttpContext[/,/]
21:32:19.619 INFO - Started org.openqa.jetty.jetty.servlet.ServletHandler@2a852e54
21:32:19.619 INFO - Started HttpContext[/wd,/wd]
21:32:19.622 INFO - Started SocketListener on 0.0.0.0:4444
21:32:19.622 INFO - Started org.openqa.jetty.jetty.Server@1dc80063
```

Now back in your **protractor** directory, run the example test. 

```
$ protractor conf.js
```

If this runs correctly, chrome should pop up and run a few tests over the AngularJS website. This means protactor has been setup properly and you should be able to use it in your testing.

Here are some references:

* [Protractor](https://github.com/angular/protractor)
* [AngularJS](https://angularjs.org/)