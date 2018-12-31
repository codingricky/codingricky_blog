+++
author = "Ricky Yim"
categories = ["Play!", "Play", "Testing", "PhantomJS"]
date = 2013-09-18T02:00:00Z
description = ""
draft = false
slug = "using-phantomjs-with-play"
tags = ["Play!", "Play", "Testing", "PhantomJS"]
title = "Using PhantomJS with Play"
aliases = [
    "/using-phantomjs-with-play/"
]
+++

Out of the box for the Play framework, you have two options for browser based testing, Firefox or HtmlUnit.<br />

Basically, you've got this test that is created for you when you generate a new Play application. Switch over the HTMLUNIT constant to &nbsp;<code>FIREFOX</code>&nbsp; to begin using Firefox through webdriver.<br />
<br />
<pre class="prettyprint linenums">    @Test
    public void test() {
        running(testServer(3333, fakeApplication(inMemoryDatabase())), HTMLUNIT, new Callback<testbrowser>() {
            public void invoke(TestBrowser browser) {
                browser.goTo("http://localhost:3333");
                assertThat(browser.pageSource()).contains("Your new application is ready.");
            }
        });
    }
</testbrowser></pre>
HtmlUnit (<a href="http://htmlunit.sourceforge.net/">http://htmlunit.sourceforge.net/</a>) does have a couple advantages over a real browser like Firefox.
<br />
<ol>
<li>It's Faster</li>
<li>It's headless, therefore doesn't need a display. Useful in CI environments like Travis (<a href="https://travis-ci.org/">https://travis-ci.org/</a>). Yes you can use Xvfb (<a href="http://en.wikipedia.org/wiki/Xvfb">http://en.wikipedia.org/wiki/Xvfb</a>) but that's just another dependency.&nbsp; Also handy locally so you don't have numerous browser windows popping up when running tests</li>
</ol>
But the biggest drawback is that the Javascript support within HtmlUnit is pretty average. I've always run into issues using it when I've done anything remotely complicated in Javascript. It uses Mozilla's Rhino (<a href="https://developer.mozilla.org/en/docs/Rhino">https://developer.mozilla.org/en/docs/Rhino</a>) under the covers.

<br />
<br />
So enter PhantomJS (<a href="http://phantomjs.org/">http://phantomjs.org/</a>). It's a headless WebKit browser with a Javascript API. Being WebKit based, I've found that it supports Javascript much better than HtmlUnit. 


So how do you integrate it into your Play application? In a few steps.

<br />
<ol>
<li>Firstly, you need to install PhantomJS. There are a number of ways to achieve this, but I just use brew (<a href="http://brew.sh/">http://brew.sh/</a>). So I run the following command.

<pre>brew install phantomjs</pre>
On the phantomjs website (<a href="http://phantomjs.org/">http://phantomjs.org/</a>) there are instructions for other Operating Systems.

</li>
<li>
Add GhostDriver (<a detro="" ghostdriver="" github.com="" href="http://www.blogger.com/blogger.g?blogID=8781280921217019185" https:="">https://github.com/detro/ghostdriver</a>) which is an implementation of WebDriver for PhantomJS. To achieve this go to your <pre>Build.scala</pre>
and add GhostDriver.

<pre class="prettyprint linenums">  val appDependencies = Seq(
    // Add your project dependencies here,
    javaCore,
    javaJdbc,
    javaEbean,
    "com.github.detro.ghostdriver" % "phantomjsdriver" % "1.0.4" % "test"
  )
</pre>
</li>
<li>Change the test to use GhostDriver.
<pre class="prettyprint linenums">    public void test() {
        running(testServer(3333, fakeApplication(inMemoryDatabase())), org.openqa.selenium.phantomjs.PhantomJSDriver, new Callback<testbrowser>() {
            public void invoke(TestBrowser browser) {
                browser.goTo("http://localhost:3333");
                assertThat(browser.pageSource()).contains("Your new application is ready.");
            }
        });
    }
</testbrowser></pre>
</li>
<li>
Re-run the test

<pre>$ play test</pre>
If everything has worked correctly, you should have output like the following

<pre>2013-09-19 11:29:06.727 phantomjs[4326:707] *** WARNING: Method userSpaceScaleFactor in class NSView is deprecated on 10.7 and later. It should not be used in new applications. Use convertRectToBacking: instead.
PhantomJS is launching GhostDriver...
[INFO  - 2013-09-19T01:29:06.832Z] GhostDriver - Main - running on port 15803
[INFO  - 2013-09-19T01:29:06.993Z] Session [e04d1ec0-20ca-11e3-89ad-45bbd56cac5d] - _decorateNewWindow - page.settings: {"XSSAuditingEnabled":false,"javascriptCanCloseWindows":true,"javascriptCanOpenWindows":true,"javascriptEnabled":true,"loadImages":true,"localToRemoteUrlAccessEnabled":false,"userAgent":"Mozilla/5.0 (Macintosh; PPC Mac OS X) AppleWebKit/534.34 (KHTML, like Gecko) PhantomJS/1.9.2 Safari/534.34","webSecurityEnabled":true}
[INFO  - 2013-09-19T01:29:06.993Z] Session [e04d1ec0-20ca-11e3-89ad-45bbd56cac5d] - page.customHeaders:  - {}
[INFO  - 2013-09-19T01:29:06.993Z] Session [e04d1ec0-20ca-11e3-89ad-45bbd56cac5d] - CONSTRUCTOR - Desired Capabilities: {"platform":"ANY","browserName":"phantomjs","version":""}
[INFO  - 2013-09-19T01:29:06.993Z] Session [e04d1ec0-20ca-11e3-89ad-45bbd56cac5d] - CONSTRUCTOR - Negotiated Capabilities: {"browserName":"phantomjs","version":"1.9.2","driverName":"ghostdriver","driverVersion":"1.0.4","platform":"mac-10.8 (Mountain Lion)-64bit","javascriptEnabled":true,"takesScreenshot":true,"handlesAlerts":false,"databaseEnabled":false,"locationContextEnabled":false,"applicationCacheEnabled":false,"browserConnectionEnabled":false,"cssSelectorsEnabled":true,"webStorageEnabled":false,"rotatable":false,"acceptSslCerts":false,"nativeEvents":true,"proxy":{"proxyType":"direct"}}
[INFO  - 2013-09-19T01:29:06.993Z] SessionManagerReqHand - _postNewSessionCommand - New Session Created: e04d1ec0-20ca-11e3-89ad-45bbd56cac5d
[INFO  - 2013-09-19T01:29:07.949Z] ShutdownReqHand - _handle - About to shutdown
[info] IntegrationTest
[info] + IntegrationTest.test
[info]
[info]
[info] Total for test IntegrationTest
[info] Finished in 0.0 seconds
</pre>
</li>
<li>You can also take screenshots with PhantomJS, which is very useful for diagnosing failures and debugging. 

<pre class="prettyprint linenums">    @Test
    public void test() {
        running(testServer(3333, fakeApplication(inMemoryDatabase())), org.openqa.selenium.phantomjs.PhantomJSDriver.class, new Callback() {
            public void invoke(TestBrowser browser) {
                browser.goTo("http://localhost:3333");
                assertThat(browser.pageSource()).contains("Your new application is ready.");
                browser.takeScreenShot("/tmp/screenshot.jpg");
            }
        });
    }
</pre>
</li>
</ol>
I've created a sample app here with it all set up <a href="https://github.com/codingricky/sample-play-with-phantomjs">https://github.com/codingricky/sample-play</a>.

Happy testing!