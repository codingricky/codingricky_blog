+++
author = "Ricky Yim"
categories = ["Testing", "Fest", "Unit Testing"]
date = 2013-09-20T02:00:00Z
description = ""
draft = false
slug = "fest-asserts"
tags = ["Testing", "Fest", "Unit Testing"]
title = "Fest Asserts"
aliases = [
    "/fest-asserts/"
]
+++

Just a quick post about the awesomeness of Fest Asserts. Fest asserts is a fluent interface for Unit testing assertions.

I've always traditionally used Hamcrest Matchers with my JUnit tests. After using Fest asserts (<a href="https://github.com/alexruiz/fest-assert-2.x">https://github.com/alexruiz/fest-assert-2.x</a>) with Play, I've been converted.<br />
<br />
&nbsp;So let me give you an example.

With Hamcrest matchers, a simple string equality assertion is performed as follows:

<br />
<pre class="prettyprint">
assertThat(client.getUser(), is("John"));
</pre>
<br />
With Fest, it is performed like this:


<br />
<pre class="prettyprint">
assertThat(client.getUser()).isEqualTo("John");
</pre>
<br />
The change between the two is very subtle, however the impact is great.<br />
<br />
&nbsp;Since Fest doesn't rely on static importing, it is much easier to 'auto-complete' Fest methods than it is to 'auto-complete' the statically imported Hamcrest matcher methods. Basically IDEs are much better at autocompleting methods belonging to Objects/Classes than methods that require static imports.

Also another consequence of not relying on static importing, is that you'll potentially run into fewer collisions with other static imports with the same name. Especially if you statically imported with '*', the chance of colliding with another method with the same name is high.<br />
<br />
&nbsp;For more information, see <a href="http://fest.easytesting.org/">http://fest.easytesting.org/</a>. There's also <a href="http://www.fluentlenium.org/">http://www.fluentlenium.org/</a> a fluent interface for selenium and <a href="https://github.com/joel-costigliola/assertj-core">assertj</a> which is a fork of fest.