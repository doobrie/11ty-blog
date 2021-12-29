---
title: Custom JDK Versions Per Project With SDKMAN!
date: 2021-07-13
tags: [java, jdk, sdkman]
splash: https://davidsalterassets.s3.eu-west-2.amazonaws.com/0_l0VmVic79yh_iMa5.jpg
splashalt: A 3D image of the word Projects
summary: SDKMAN! allows us to easily install and use different JDKs. It can become frustrating to have to change JDK versions manually for different projects. Fortunately, SDKMAN! has a solution to this in the form of the `sdk env` command. This command allows us to define a different JDK per project which can be easily switched to, without having to remember the JDK version. Additionally, it can be configured to automatically change to the required JDK when changing directories.
---

[SDKMAN!](https://sdkman.io) allows us to easily install and use different JDKs. It can become frustrating to have to change JDK versions manually for different projects. Fortunately, SDKMAN! has a solution to this in the form of the `sdk env` command. This command allows us to define a different JDK per project which can be easily switched to, without having to remember the JDK version. Additionally, it can be configured to automatically change to the required JDK when changing directories.

For more information about installing and using SDKMAN!, check out my article - [Managing JDK Versions With SDKMAN!](https://www.davidsalter.com/posts/managing-jdk-versions-with-sdkman/)

## Updating SDKMAN!

Before we can start using the `sdk env` command, we must first ensure we are using the latest version of SDKMAN! The software can be updated by using the `sdk selfupdate` command

```bash
% sdk selfupdate
No update available at this time.
```

## Specifying a JDK Version For A Project

SDKMAN! uses a hidden file called `.sdkmanrc` which contains a parameter specifying which JDK version to use for a project. To configure and use this file is a simple 3 step process.

1 **Create the `.sdkmanrc` file**. This can easily be created by navigating to the required directory and executing `sdk env init`

```bash
% sdk env init
.sdkmanrc created.
```

2 **Configure the version**. Once we've created the file, edit it and add a line `java=x.x.x.x` specifying the version of Java to use. For example to use Adopt Open JDK version 11, the file would look like:

```bash
% cat .sdkman
java=11.0.2.hs-adpt
```

3 **Initialize the environment**. After editing the `.sdkmanrc` fille, we can configure the environment by executing the `sdk env` command.

```bash
% sdk env

Using java version 11.0.2-open in this shell.
%
% java -version
openjdk version "11.0.2" 2019-01-15
OpenJDK Runtime Environment 18.9 (build 11.0.2+9)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.2+9, mixed mode
```

## Specifying a JDK Automatically

Finally, we can get SDKMAN! to automatically change JDK to the version defined within `.sdkmanrc` by changing configuration. To do this, edit the `~/.sdkman/etc/config` file and add/edit the property `sdkman_auto_env=true`. This will probably already exist and be set to `false`, so make sure you edit this setting if it is there rather than duplicating it.

Now, whenever you change into a directory containing a `.sdkmanrc` file in it, the JDK will automatically be changed to your desired version. Fantastic !

```bash
% cd project1

Using java version 11.0.2-open in this shell.
%
% cd project2

Using java version 14.0.1-open in this shell.
```

## Credits

Photo by [Octavian Dan](https://unsplash.com/@octadan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
