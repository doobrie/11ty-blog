---
title: Managing JDK Versions With SDKMAN!
date: 2021-07-08
tags: [java, jdk, sdk, sdkman]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628544091/0_mKjmWsedAOBUwsoP_lptobo.jpg)

SDKMAN! describes itself as:

> a tool for managing parallel versions of multiple Software Development Kits on most Unix based systems. It provides a convenient Command Line Interface (CLI) and API for installing, switching, removing and listing Candidates.

What this means, is that it is a powerful tool that allows you to easily change between different versions of the JDK without having to modify `JAVA_HOME` or other environment variables.

## Installing SDKMAN!

Installation is as simple as running a bash command. For Linux and Mac users, simply execute the following command and follow the on-screen instructions.

```bash
curl -s "https://get.sdkman.io" | bash
```

For Windows users, the recommended way of installation is via WSL. This is covered in the [installation documentation](https://sdkman.io/install).

## Installing a JDK

After installing SDKMAN!, the next stage is to decide which JDK versions you wish to install. SDKMAN! allows you to install different version of the JDK by different vendors. To get a complete list of the different JDK's available, execute the `sdk list java` command:

```bash
% sdk list java
================================================================================
Available Java Versions
================================================================================
 Vendor        | Use | Version      | Dist    | Status     | Identifier
--------------------------------------------------------------------------------
 AdoptOpenJDK  |     | 14.0.1.j9    | adpt    |            | 14.0.1.j9-adpt
               |     | 14.0.1.hs    | adpt    |            | 14.0.1.hs-adpt
               |     | 13.0.2.j9    | adpt    |            | 13.0.2.j9-adpt
               |     | 13.0.2.hs    | adpt    |            | 13.0.2.hs-adpt
               |     | 12.0.2.j9    | adpt    |            | 12.0.2.j9-adpt
               |     | 12.0.2.hs    | adpt    |            | 12.0.2.hs-adpt
               |     | 11.0.7.j9    | adpt    |            | 11.0.7.j9-adpt
               |     | 11.0.7.hs    | adpt    |            | 11.0.7.hs-adpt
               |     | 8.0.252.j9   | adpt    |            | 8.0.252.j9-adpt
               |     | 8.0.252.hs   | adpt    |            | 8.0.252.hs-adpt
 Amazon        |     | 11.0.7       | amzn    |            | 11.0.7-amzn
               |     | 8.0.252      | amzn    |            | 8.0.252-amzn
               |     | 8.0.202      | amzn    |            | 8.0.202-amzn
...
```

This command will list all the different vendors and all the different JDK's available from each vendor.

In the snippet above, you can see, for example, that Amazon provides (amongst others) Java version 11.0.7 and AdoptOpenJDK supports version 14.0.1 (again amongst others).

To install a specific JDK, we use the `sdk install java` command, noting the Identifier column. This is used to uniquely specify vendor and JDK version.

So, for example to install AdoptOpenJDK version 14, we would execute `sdk install java 14.0.1.hs-adpt`

```bash
% sdk install java 14.0.1.hs-adpt

Downloading: java 14.0.1.hs-adpt

In progress...

#################### 100.0%
#################### 100.0%

Repackaging Java 14.0.1.hs-adpt...

Done repackaging...
Cleaning up residual files...

Installing: java 14.0.1.hs-adpt
Done installing!

Do you want java 14.0.1.hs-adpt to be set as default? (Y/n): y

Setting java 14.0.1.hs-adpt as default.
```

We can now check the Java version:

```bash
 % java -version
openjdk version "14.0.1" 2020-04-14
OpenJDK Runtime Environment AdoptOpenJDK (build 14.0.1+7)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 14.0.1+7, mixed mode, sharing)
```

In this example, we selected to install this version of Java as the default. What if we just want to specify a different version of the JDK to use rather than installing a version?

## Changing JDK Version

To use a specific JDK version, you must first install it as shown in the previous section. Then, simply execute the `sdk use java ...` command to change the current shell to be a specific version.

```bash
% sdk use java 11.0.7.hs-adpt

Using java version 11.0.7.hs-adpt in this shell.
david@Davids-MacBook-Air ~ % java -version
openjdk version "11.0.7" 2020-04-14
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.7+10)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.7+10, mixed mode)
```

If we want to permanently change the JDK version, we can use the `sdk default java ...` command

```bash
% sdk default java 11.0.7.hs-adpt

Default java version set to 11.0.7.hs-adpt
```

Now, when we open a new shell or terminal, the default JDK will have changed to the one we just specified. We can use the `sdk current java` command to display the currently selected JDK version.

## Its not Just Java though!

Throughout this article, we've made reference to specifying different versions of the Java JDK. SDKMAN! is much more than a tool to change Java versions - it can do the same job for different SDK's and tools, for example, Apache Ant, Maven or Spring Boot.

To get a full list of managed SDKs, execute `sdk list`, or visit the prioject [documentation](https://sdkman.io/sdks).

## Conclusion

In this article, we've seen how to install SDKMAN! and how to install and select different JDK versions. This is only one way of defining different JDKs, but its very powerful and very simple.

## Credits

Photo by [Tom Hermans](https://unsplash.com/@tomhermans?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
