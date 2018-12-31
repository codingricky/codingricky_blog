+++
author = "Ricky Yim"
categories = ["marvel", "rest", "maven", "java", "api"]
date = 2014-02-12T11:28:36Z
description = ""
draft = false
slug = "marvels-superheroic-rest-api"
tags = ["marvel", "rest", "maven", "java", "api"]
title = "Marvel's Superheroic REST Api"
aliases = [
    "/marvels-superheroic-rest-api/"
]
+++

Recently Marvel comics released a [REST api](https://developer.marvel.com/) that allows developers to access comic book characters and their associated comics and stories. I know how cool is this! This post will take you through some sample code and show you how to get information back from their API.

Here are the pre-requistes needed before proceeding.

* Java 1.6 and above
* Maven 3
* An account on https://developer.marvel.com. This should give you a public/private key combination

Firstly, setup a pom.xml file for maven and setup two dependencies. We will use [resty](http://beders.github.io/Resty/Resty/Overview.html) as our HTTP client and Apache's [common codec](http://commons.apache.org/proper/commons-codec/) to simplify the process of hashing our parameters. Create a pom.xml file that looks like the following.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.github.codingricky</groupId>
    <artifactId>marvel-rest-client</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>marvel-rest-client</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>us.monoid.web</groupId>
            <artifactId>resty</artifactId>
            <version>0.3.2</version>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.9</version>
        </dependency>
    </dependencies>
</project>

```

Now that the dependencies are set, let's create a simple REST client that exercises one of the services. Realistically if you can interact with one end point, you should be able to apply the same technique to interact with the others.

## Understanding the URL structure

From the [documentation](http://developer.marvel.com/documentation/), the url to get all the characters is this http://gateway.marvel.com/v1/public/characters. We can't call this directly, we will need to pass in a few extra parameters for authorisation purposes.

Our url needs to look something like this:

http://gateway.marvel.com/v1/public/characters?timeStamp=**timeStamp**&apiKey=**apiKey**&hash=**hash**

Except the parameters need to be replaced with real values.

Let's breakdown the extra parameters we are passing in.

<table>
<tr>
	<td>Parameter</td>
	<td>Value</td>
</tr>
<tr>
	<td>timeStamp</td>
    <td>a timestamp (or other long string which can change on a request-by-request basis)</td>
</tr>
<tr>
	<td>apiKey</td>
    <td>Your Public Key</td>
</tr>
<tr>
	<td>hash</td>
    <td>md5 hash of the following fields concatenated (timeStamp + privateKey + publicKey)</td>
</tr>
</table>

Given the fact that we understand how requests need to be formulated, let's create a very simple REST client that invokes this service.

##REST Client

Let's create the src directory structure.

```shell
mkdir -p src/main/java
```

Now create a file with the path *src/main/java/RestClient.java* with the following contents.

```java
import java.io.IOException;
import java.security.NoSuchAlgorithmException;

import org.apache.commons.codec.digest.DigestUtils;
import us.monoid.web.Resty;

public class RestClient {
    public static void main(String[] args) throws IOException {
        String publicKey = args[0];
        String privateKey = args[1];
        long timeStamp = System.currentTimeMillis();
        int limit = 5;

        String stringToHash = timeStamp + privateKey + publicKey;
        String hash = DigestUtils.md5Hex(stringToHash);

        String url = String.format("http://gateway.marvel.com/v1/public/characters?ts=%d&apikey=%s&hash=%s&limit=%d", timeStamp, publicKey, hash, limit);
        String output = new Resty().text(url).toString();
        System.out.println(output);
    }
}
```

The following points need to be made about the code above.

 * The public key and private key are passed into the client program as the first and second argument respectively
 * The timestamp used is just the System time, so *System.currentTimeMillis()*
 * The DigestUtils from *common-codec* is used to hash the concatenated string
 * A limit of 5 is set to restrict the amount of data coming back
 * A simple text string is returned from the request and printed out to the console
 
With all this done, you should be able to compile and run it.

```shell
$ mvn clean install
$ java -classpath target/*.jar RestClient <your public key> <your private key> 
```

If this runs correctly, you should receive output like the following. I've truncated the output for brevity's sake.

```shell
{"code":200,"status":"Ok","etag":"3c514a4e7be956726463f441e6f28a64c23b12e6","data":{"offset":0,"limit":5,"total":1402,"count":5,"results":[{"id":1009521,"name":" Hank Pym",
...

```

Hopefully this post showed you the first steps towards integrating with the Marvel REST API. Good luck with making your Marvel super hero charged applications!

If you need any further assistance, the Marvel [documentation](http://developer.marvel.com/documentation/) is actually quite comprehensive and should be your first port of call.



