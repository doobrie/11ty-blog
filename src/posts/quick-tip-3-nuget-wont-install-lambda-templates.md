---
title: Quick Tip No 3 - Nuget Won't Install Lambda Templates
date: 2021-04-11
tags: [tip, node, dropbox]
---

![](/images/quick.jpg)

You're trying to install the AWS Lambda Templates to use with `dotnet`, and its not working. Why ?

```bash
@David ➜ dev dotnet new -i "Amazon.Lambda.Templates::*"
  Determining projects to restore...
C:\Users\David\.templateengine\dotnetcli\v5.0.202\scratch\restore.csproj : error NU1101: Unable to find package Amazon.Lambda.Templates. No packages exist with this id in source(s): Microsoft Visual Studio Offline Packages
  Failed to restore C:\Users\David\.templateengine\dotnetcli\v5.0.202\scratch\restore.csproj (in 56 ms).
```

Looking at the error, we can see that the package we've requested to install doesn't exist. So how do we fix that ?

Chances are that you haven't got a properly configured `nuget.config` file, so let's create one. To create such a file, execute the following command:

```bash
dotnet new nugetconfig
```

This will create a `nuget.config` file in your current directory with the following contents:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
    <add key="nuget" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
```

This creates a local file, so to be able to use it globally, copy it into the `c:\Program Files(86)\Nuget\Config` folder.

```bash
copy .\nuget.config 'C:\Program Files (x86)\NuGet\Config\'
```

You should now be able to install the Lambda templates:

```bash
dotnet new -i "Amazon.Lambda.Templates::*"
  Determining projects to restore...
  Restored C:\Users\David\.templateengine\dotnetcli\v5.0.202\scratch\restore.csproj (in 944 ms).

Templates                              Short Name                                    Language    Tags
-------------------------------------  --------------------------------------------  ----------  ----------------------
Order Flowers Chatbot Tutorial         lambda.OrderFlowersChatbot                    [C#]        AWS/Lambda/Function
Lambda Custom Runtime Function (.N...  lambda.CustomRuntimeFunction                  [C#],F#     AWS/Lambda/Function
Lambda Detect Image Labels             lambda.DetectImageLabels                      [C#],F#     AWS/Lambda/Function
...
```

## Credits

Photo by <a href="https://unsplash.com/@the_gerbs1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jean Gerber</a> on <a href="https://unsplash.com/@the_gerbs1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
