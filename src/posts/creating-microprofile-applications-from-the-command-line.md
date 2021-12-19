---
title: Creating MicroProfile Applications From The Command Line
date: 2021-07-13
tags: [java, microprofile]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628543358/0_sxxGhVh0aHqTPmZT_agofcp.jpg)

## Introduction

The [MicroProfile](https://microprofile.io/) initiative provides the excellent web based [Starter](https://start.microprofile.io/) application to allow developers to easily scaffold MicroProfile based applications.

## MicroProfile Starter REST Interface

Recently, a new version of the Starter has been published that exposes a REST interface allowing MicroProfile applications to be easily created from the command line. In this post, we’ll show how this can be used.

## Supported Servers

To create a project with the default settings, a HTTP GET request must be made to the following url: `https://start.microprofile.io/api/1/project?supportedServer=<SERVER>`

This url takes as a minimum one parameter, `<SERVER>`, which specifies which application server (that supports MicroProfile) to use.

So, how do we get a valid `<SERVER>`. Well, not all application servers provide the same level of support for different versions on MicroProfile, so first, we must decide which version of MicroProfile we wish to target. Once we know that, we can select an appropriate application server and then create a project.

The first stage therefore, is to get a list of different versions of MicroProfile. We can then query the service to establish which application servers support the requested version.

## Supported Versions

To get a list of MicroProfile versions, we must perform a HTTP GET request to `https://start.microprofile.io/api/1/mpVersion`

```bash
david ~ $ curl https://start.microprofile.io/api/1/mpVersion
["MP22","MP21","MP20","MP14","MP13","MP12"]
```

This tells us that there are currently 6 versions of MicroProfile that are supported by the REST service: from MicroProfile 1.2 through to MicroProfile 2.2

As different application servers support different versions of MicroProfile, we can query the service to establish which application servers are supported for a specified version of MicroProfile. For example, to get a list of application servers that support MicroProfile 2.0, we would execute the following command:

```bash
david ~ $ curl https://start.microprofile.io/api/1/mpVersion/MP20
{"supportedServers":["KUMULUZEE","LIBERTY","PAYARA_MICRO","TOMEE"],"specs":["CONFIG","FAULT_TOLERANCE","JWT_AUTH","METRICS","HEALTH_CHECKS","OPEN_API","OPEN_TRACING","REST_CLIENT"]}
This api call tells us that Kumuluzee, Open Liberty, Payara Micro and TomEE support version 2.0 of MicroProfile and they support the MicroProfile APIs:
Fault Tolerance
JWT Authentication
Metrics
Health Checks
Open API
Open Tracing
REST Client
```

We now have sufficient information to create a basic MicroProfile 2.0 application. In this example, we’ll use the TomEE application server.

## Creating an Application

So, to create a basic MicroProfile application, execute the following command:

```bash
david ~ $ curl -O -J https://start.microprofile.io/api/1/project?supportedServer=TOMEE
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 12555  100 12555    0     0  44818      0 --:--:-- --:--:-- --:--:-- 44679
curl: Saved to filename 'demo.zip'
```

The `-O` parameter tells curl to download a file rather than output to the console. The `-J` parameter tells curl to correctly name the file after it is downloaded, in this case, `demo.zip`. If the -J parameter was not specified, the downloaded file would be called `project?supportedServer=TOMEE`

So, what does the created project look like? A bit like this :)

```bash
demo/src/test/java/.gitkeep
demo/src/main/webapp/WEB-INF/beans.xml
demo/src/main/java/com/example/demo/HelloController.java
demo/pom.xml
demo/src/main/java/com/example/demo/health/ServiceHealthCheck.java
demo/src/main/resources/META-INF/microprofile-config.properties
demo/src/main/java/com/example/demo/resilient/ResilienceController.java
demo/src/main/webapp/.gitkeep
demo/src/test/java/com/example/demo/JWTClient.java
demo/src/main/java/com/example/demo/config/ConfigTestController.java
demo/src/test/resources/privateKey.pem
demo/src/main/java/com/example/demo/DemoRestApplication.java
demo/readme.md
demo/src/main/resources/.gitkeep
demo/src/main/java/com/example/demo/secure/ProtectedController.java
demo/src/test/java/com/example/demo/MPJWTToken.java
demo/src/main/resources/publicKey.pem
demo/src/main/webapp/index.html
demo/src/main/java/com/example/demo/metric/MetricController.java
```

The first thing you notice here is that the package names are of the format `com.example.demo`. We can easily refactor this in an IDE, or we could specify the Maven Group and Artifact Id’s and have the project created exactly as we want it, by appending the `groupId` and `artifactId` arguments to the call.

For example, to create the project with a `groupId` of `com.acme` and an `artifactId` of `customer-service`, we would append the `groupId` and `artifactId` parameters as:

```bash
david ~ $ curl -O -J 'https://start.microprofile.io/api/1/project?supportedServer=TOMEE&groupId=com.acme&artifactId=customer-service'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 13420  100 13420    0     0  45698      0 --:--:-- --:--:-- --:--:-- 45802
curl: Saved to filename 'customer-service.zip'
```

Now, note that the download is called `customer-service.zip` and its contents are:

```bash
customer-service/src/main/java/com/acme/customer/service/metric/MetricController.java
customer-service/src/main/java/com/acme/customer/service/resilient/ResilienceController.java
customer-service/pom.xml
customer-service/src/main/java/com/acme/customer/service/CustomerserviceRestApplication.java
customer-service/src/main/webapp/index.html
customer-service/src/main/webapp/WEB-INF/beans.xml
customer-service/src/main/java/com/acme/customer/service/HelloController.java
customer-service/src/main/java/com/acme/customer/service/config/ConfigTestController.java
customer-service/src/main/webapp/.gitkeep
customer-service/src/main/resources/META-INF/microprofile-config.properties
customer-service/src/main/resources/.gitkeep
customer-service/src/test/resources/privateKey.pem
customer-service/src/test/java/com/acme/customer/service/JWTClient.java
customer-service/src/main/java/com/acme/customer/service/health/ServiceHealthCheck.java
customer-service/src/main/resources/publicKey.pem
customer-service/src/main/java/com/acme/customer/service/secure/ProtectedController.java
customer-service/src/test/java/.gitkeep
customer-service/readme.md
customer-service/src/test/java/com/acme/customer/service/MPJWTToken.java
```

With the starter web application, we can also request to add examples for the different MicroProfile specifications (such as Config or Fault Tolerance) into the generated application. We can also do this with the REST client by utilizing the `selectedSpecs` parameter. The values used for this parameter are those returned in the curl shown at the beginning of this post.

So for example, to add an example for Config and Fault Tolerance, we would use the following curl:

```bash
david ~ $ curl -O -J 'https://start.microprofile.io/api/1/project?supportedServer=TOMEE&groupId=com.acme&artifactId=customer-service&selectedSpecs=CONFIG&selectedSpecs=FAULT_TOLERANCE'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  6107  100  6107    0     0  17580      0 --:--:-- --:--:-- --:--:-- 17548
curl: Saved to filename 'customer-service.zip'
```

In this post, we’ve seen how we can easily utilize the MicroProfile Starter REST interface to easily create projects.

Happy MicroProfiling !

## Credits

Photo by [Athul Cyriac Ajay](https://unsplash.com/@athulca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&
