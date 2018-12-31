+++
author = "Ricky Yim"
date = 2015-03-30T23:52:30Z
description = ""
draft = false
slug = "garmins-connect-iq"
title = "Introduction to Garmin's Connect IQ"
aliases = [
    "/garmins-connect-iq/"
]
+++

Late last year, Garmin announced a development platform for their wearables. This enabled software developers to build applications for Garmin watches. This opens up a whole host of possibilties in how people use their watches. This article will explain the various types of applications that can be built for the platform.

Currently there are four watches which support the Connect IQ platform (920XT, Epix, Fenix 3 and Vivoactive). Garmin defines four distinct types of applications that can be developed, watch faces, data fields, widgets and apps.  I will describe each one.

###### What can you build?

![](/images/2015/03/application-types.png)

1.Watch Faces

Watch faces are designed to tell the time. They are restrictive as they can not access the GPS or use BLE to connect to the internet via a phone. They should be always-on hence the forementioned restrictions. This helps converse the battery of the watch.

2.Data Fields

Data Fields are used in activities to display to the user what is occuring in the activity. Data fields could be speed, distance and time. A custom data field can take any of these data fields and produce a new field all together. One of the first custom data fields that is often written is a calorie to beer conversion showing users how many beers they have burned during their activity!

3.Widgets

Similar to widgets in the iOS and Android land, widgets in the Garmin world are designed to show information at a glance to the user. They are a "swipe" away from the watch face. They are allowed to use the GPS and the phone to access the internet as they are not meant to be on all the time. Garmin provides a weather widget and a music remote widget to control the music on your phone.

4.Apps

These are the least restrictive type of applications that you can build. They need to be explicitly launched. They have access to everything that is possible on the phone from the accelerometer to the GPS to the internet. You can access Heart Rate Monitors and Powermeters via ANT+ and BLE.

###### How to distribute your apps?

Garmin have setup the Connect IQ Store to distribute your apps. There is no monetization model behind the store so all apps are distributed free. Garmin do suggest that you can charge for web services but this is handled outside their app store. 

This article gave you a quick introduction into the types of apps that can be built for the Connect IQ platform. The next article in this series will explore the development tools and build a very simple application.

Here are some references:

* [Garmin Connect IQ APIs](http://developer.garmin.com/connect-iq/overview)
* [Garmin Connect IQ Store](https://apps.garmin.com)


