+++
author = "Ricky Yim"
categories = ["maven", "faker", "maven shade"]
date = 2016-05-07T01:50:38Z
description = ""
draft = false
slug = "keeping-it-all-together-with-the-maven-shade-plugin"
tags = ["maven", "faker", "maven shade"]
title = "Keeping it all together with the Maven Shade Plugin"
aliases = [
    "/keeping-it-all-together-with-the-maven-shade-plugin/"
]
+++

[Java Faker](https://github.com/DiUS/java-faker) is an open source project that I maintain. It is a port of the Ruby faker gem and it exists to generate fake but realistic data. So we can insert phrases like *"The fool doth think he is wise, but the wise man knows himself to be a fool"* into our application instead of *"Lorem ipsum have a nice day."*

One of the things that is constantly on my mind is how to keep the bar to use the library as low as possible. This is to keep its adoption rate as high as possible. Hence why in travis I still have it compiling against Java 1.6. 

A constant feature that I get asked about is Android support. And the one thing that holds it back is the YAML parser used. It's [SnakeYAML](https://bitbucket.org/asomov/snakeyaml) and it has a dependency to javabeans which does not exist in Android Java land therefore meaning the Faker library doesn't work on Android. 

However I recently discovered a fork of the [SnakeYAML](https://github.com/bmoliveira/snake-yaml) that supports Android. My initial thinking was to simply replace the library with the Android compliant version, run tests and if everything passed, make the change and move on. However the downside was that the jar file wasn't published to the central maven repository. So as one [committer](https://github.com/PascalSchumacher) to the project highlighted to me this could cause grief for people using the library who worked in environments that blocked access to non standard maven repositories like http://jitpack.io. This committer also provided a solution. He suggested the [Maven Shade plugin](https://github.com/PascalSchumacher).

What this plugin does is provide the option to encapsulate multiple jars together to publish an even bigger jar at the end. By including the Android SnakeYAML jar at build time means that users of the faker jar don't need to download it themselves.

Here are the steps to achieve this. Youa add the maven shade plugin to your pom.xml and tell it which dependency you want to include. Simple as that.

```
 <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.4.3</version>
                <configuration>
                    <artifactSet>
                        <includes>
                            <include>com.github.bmoliveira:snake-yaml</include>
                        </includes>
                    </artifactSet>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

Now if you proceed to run **mvn package** your jar file should be bigger and contain an extra jar file in there. 

I hope this tip was handy for you. It's a simple one that may be useful in your travels. 