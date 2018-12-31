+++
author = "Ricky Yim"
categories = ["QRcode", "RubyMotion", "iOS", "Prezi", "Rails", "Heroku"]
date = 2014-05-09T08:09:24Z
description = ""
draft = false
slug = "myhealth-hack-day-2"
tags = ["QRcode", "RubyMotion", "iOS", "Prezi", "Rails", "Heroku"]
title = "MyHealth - Hack Day #2"
aliases = [
    "/myhealth-hack-day-2/"
]
+++

On the 28th of March, my company [DiUS computing](http://www.dius.com.au), held their second Hack Day. This time the theme was around "creating shared value." So hacking on something that provided value to the community as well as DiUS at the same time. Other rules were simple, teams had to consist of at least 5 people, all software written was to be open sourced and by 4:30pm of that day, teams were to run a "marketplace" that demo-ed their working software. 

After a few conversations with various parties, I joined the MyHealth team that looked at tackling the issue of online medical records. What we decided to focus on was having a wearable QR code that uniquely identified a person's medical information. The scenario this targeted was the wearer being found unconcious and upon scanning the QR code, would take the person scanning the code to vital personal medical information that could save the wearer's life.

These are some choices and technical decisions that were made throughout the day.

* Settled on using Rails basically because we wanted to push out something demonstrable quickly and Rails amongst other things, is very good at achieving that goal
* We setup [github](http://www.github.com) for SCM, [travis](http://travis-ci.org) for Continuous Integration and [heroku](https://www.heroku.com/) for Continuous Delivery. These tools are excellent for setting up collaborative environments for developers 
* We used [ActiveAdmin](http://activeadmin.info/) to bootstrap our CRUD screens
* [RubyMotion](http://www.rubymotion.com) for the iOS stream. This allows for Ruby developers to develop iOS software without having to learn Objective-C 
* [Twitter Bootstrap](http://getbootstrap.com/) for the medical information page because we wanted a responsive design such that the page was viewable irrespective of device
* [Prezi](https://prezi.com/) as a presentation tool to capture our pitch which can be found [here](http://prezi.com/jns9rnkingxa/?utm_campaign=share&utm_medium=copy)

So we ended up with a demonstrable piece of software which would generate QR codes like this: 

![](/content/images/2014/May/BjyKRcgCAAAKZzN.png)

Upon scanning it in, it would take the scanner to a page like this: 

![](/content/images/2014/May/MyHealth.png)


The source code to this can be found [here](https://github.com/DiUS/myhealth). 

The mobile component of our solution used RubyMotion to create a small iOS application that allowed one to set their lock screen to contain their emergency information. The idea behind this was to allow someone to take appropriate action upon finding one's phone and not requiring them to unlock it.

![](/content/images/2014/May/screen2.jpg)

The source code to this can be found [here](https://github.com/DiUS/myhealth-ios).

Overall, I had an awesome time and I am eagerly awaiting the next Hack Day.

