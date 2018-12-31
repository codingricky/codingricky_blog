+++
author = "Ricky Yim"
categories = ["java", "jdk", "jenv"]
date = 2014-03-06T23:34:10Z
description = ""
draft = false
slug = "jenv"
tags = ["java", "jdk", "jenv"]
title = "Running Multiple JDKs on OSX"
aliases = [
    "/jenv/"
]
+++

I often need to have multiple versions of Java installed on my machine. Some projects I work on use an older version of Java (for e.g, 1.6), whilst I also like to dabble with the latest and greatest version to see what features are looming on the horizon. As I use OSX extensively, I have traditionally fulfilled this requirement by using Virtual Machines either through [vmware fusion](http://www.vmware.com) or through [virtual box](https://www.virtualbox.org/). However, the biggest downfall with this approach is the performance of running VMs as they can be quite resource intensive. Also, I like working in a native OSX environment, and I don't really like the context switching that occurs when using a VM. 

Recently, I came across [jENV](http://www.jenv.be/) which I describe as [rbenv](https://github.com/sstephenson/rbenv) or [RVM](https://rvm.io/) for Java. Essentially it allows developers to switch between different versions of Java as required. So the rest of this article will take you through the very simple steps of how to set [jENV](http://www.jenv.be/) up.

The following instructions are for OSX and they assume you have [brew](http://brew.sh/) installed. 

Firstly, you need to install jENV from brew. 

Technically, you need git installed first, but I would imagine most people would have git already.

```shell
$ brew install git
```

```shell
$ brew tap jenv/jenv
$ brew install jenv
```

Next thing you will need to do is update your .bash_profile script, that is assuming you use bash.

```shell
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(jenv init -)"' >> ~/.bash_profile
```

Now if everything has worked properly, you should be able to start adding new JDKs and configuring the current Java version as shown below. Make sure the paths below point to JDKs that you have installed.

``` shell
$ jenv add /System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0.jdk/Contents/Home
$ jenv global oracle64-1.8.0-ea
$ jenv versions

  system
  oracle64-1.6.0.65
* oracle64-1.8.0-ea (set by /Users/ricky/.jenv/version)
$ java -version
java version "1.8.0-ea"
Java(TM) SE Runtime Environment (build 1.8.0-ea-b108)
Java HotSpot(TM) 64-Bit Server VM (build 25.0-b50, mixed mode)

$ jenv global oracle64-1.6.0.65
$ java -version
java version "1.6.0_65"
Java(TM) SE Runtime Environment (build 1.6.0_65-b14-462-11M4609)
Java HotSpot(TM) 64-Bit Server VM (build 20.65-b04-462, mixed mode)
```

For more information, please check out the official site [here](http://jenv.be). 
