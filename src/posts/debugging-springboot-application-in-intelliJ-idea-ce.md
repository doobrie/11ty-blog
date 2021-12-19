---
title: Debugging SpringBoot Application In IntelliJ Idea CE
date: 2021-07-07
tags: [aws, amplify]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628716593/0_Xam9IE06KMzgp19E_f7p3zs.jpg)

When debugging a SpringBoot application in IntelliJ Idea Community Edition, additional steps need to be taken.

If you have defined your run configuration as `spring-boot:run`, you will find that the application runs, but does not stop at breakpoints as expected.

An easy way to resolve this is to set the `spring-boot.run.fork` property to `false`.

This can be set wither within the `pom.xml` file, or within the run configuration. To set the run configuration, set the "Command line:" within the Run Configuration to be:

```bash
spring-boot:run -Dspring-boot.run.fork=false
```

The default value of this property is `true`, which is usually the best setting, but not when debugging from IntelliJ Idea CE. If the application is forked, then the relevant JVM properties are not passed on to the forked process and debugging does not work as expected.

Changing the value of this property all the time can cause problems with Spring Boot DevTools, so its only recommended to set at runtime and not in the `pom.xml` file where it will be used every time the application is started.

## Credits

Photo by [Zan](https://unsplash.com/@zanilic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
