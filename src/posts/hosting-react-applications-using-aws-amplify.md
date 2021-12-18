---
title: Hosting React Applications Using AWS Amplify
date: 2021-03-17
tags: [aws, amplify, react]
---

![](/images/hosting.jpg)

## Introduction

So, you've written "The Next Best Thing" - how do you host it? There are many different ways to host an application, but [AWS Amplify](https://aws.amazon.com/amplify/) provides a simple, yet powerful way of hosting apps.

With AWS Amplify, you can start easily by hosting your applications in a S3 bucket, and then can easily add extra features such as authentication or back-end DynamoDB databases. In this article, I'm going to show you how to get started with AWS Amplify and host "The Next Best Thing".

<!-- more -->

![next-best-thing.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1616015988588/52pcnRbq2.png)

## Sample Application

The sample application I'm going to deploy is pretty much the stock application created by `create-react-app`, but with different text. You can see a screen shot of it above. But we're not here to learn how to develop in React (although that's an interesting subject), we're here to see how easy it is to deploy a React app using AWS Amplify

## Getting Set Up

To get started, we need to ensure we have an AWS account and Node.js installed (at least version 10.x) with either `npm` or `yarn`. I'm going to assume you're using `yarn`.

To install the AWS Amplify CLI execute the following command:

```
yarn global add @aws-amplify/cli
```

After installing the cli, we need to configure it to give it details of our default AWS region to use and appropriate IAM credentials to be able to provision services within the AWS cloud. This is achieved by executing:

```
amplify configure
```

Running this command will allow you to create a profile that has administrative and programmatic rights to AWS. Simply answer the questions displayed by the wizard. You only need to do this once as a local profile will be created for future use.

## Configuring Your Application

Now we've got everything setup, we can go ahead and use AWS Amplify to add hosting to our application. The first stage is to initialize AWS Amplify, so from a command prompt / terminal opened up within your project's directory, execute

```
amplify init
```

You'll now be asked several questions about your application, such as what's the name of the application, what's the language and framework, where is the code and how do you run it. In the example below, you can see that I'm hosting an JavaScript React project called `nextbestthing`

```text
$ amplify init
Note: It is recommended to run this command from the root of your app directory
? Enter a name for the project nextbestthing
? Enter a name for the environment dev
? Choose your default editor: Visual Studio Code
? Choose the type of app that you're building javascript
Please tell us about your project
? What javascript framework are you using react
? Source Directory Path:  src
? Distribution Directory Path: build
? Build Command:  npm run-script build
? Start Command: npm run-script start
Using default provider  awscloudformation
```

Having specified details of your project, you'll be asked what profile you want to use for deployment. This is the profile we configured earlier which specifies what IAM user is allowed to deploy our application.

```text
? Select the authentication method you want to use: AWS profile

For more information on AWS Profiles, see:
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html

? Please choose the profile you want to use amplify-admin
```

Upon specifying the profile, AWS Amplify will get to work provisioning the back end services necessary for your application. "But we're not using any back-end services yet?" I hear you say. That's correct, but Amplify needs to create a S3 bucket for provisioning using CloudFormation.

```text
CREATE_IN_PROGRESS amplify-nextbestthing-dev-213949 AWS::CloudFormation::Stack Wed Mar 17 2021 21:39:58 GMT+0000 (Greenwich Mean Time) User Initiated
⠹ Initializing project in the cloud...

CREATE_IN_PROGRESS DeploymentBucket AWS::S3::Bucket Wed Mar 17 2021 21:40:02 GMT+0000 (Greenwich Mean Time) Resource creation Initiated
⠏ Initializing project in the cloud...

...

Your project has been successfully initialized and connected to the cloud!
```

## Deploying The Application

We're almost there !

To deploy our application, we again use the amplify cli. First of all we need to configure the project and add hosting to it. This is done by executing the command `amplify add hosting`.

```
$ amplify add hosting
? Select the plugin module to execute Amazon CloudFront and S3
? Select the environment setup: DEV (S3 only with HTTP)
? hosting bucket name nextbestthing-20210317214757-hostingbucket
? index doc for the website index.html
? error doc for the website index.html

You can now publish your app using the following command:
Command: amplify publish
```

As you'd expect, Amplify asks a few questions to finely tune how we want to deploy the application.

The first question is how do we want to host our application. We get the choice of:

- Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment)
- Amazon CloudFront and S3.

Since this is our first steps into hosting, I'm choosing the `Amazon CloudFront and S3` option as this will allow the application to be easily hosted via HTTP from a S3 bucket.

Next we get asked to specify the environment (either Dev, or Prod). The main differences here are that Dev uses HTTP and Prod uses CloudFront to provide HTTPS.

Next up is specifying the name of the S3 bucket to use for hosting.

Finally, we need to specify the index and error pages for the application. Since we're deploying a React application, we'll use the default of index.html for both of these.

Now we've answered all of the Amplify's questions, we can deploy the application. This is achieved by executing `amplify publish`

```
$ amplify publish
✔ Successfully pulled backend environment dev from the cloud.

Current Environment: dev

| Category | Resource name   | Operation | Provider plugin   |
| -------- | --------------- | --------- | ----------------- |
| Hosting  | S3AndCloudFront | Create    | awscloudformation |
? Are you sure you want to continue? Yes
⠇ Updating resources in the cloud. This may take a few minutes...

...

Hosting endpoint: http://nextbestthing...

...

Your app is published successfully.
http://nextbestthing...
```

And that's it. Your application is now live on the internet.

## Summary

In this post, we've seen how to install and configure AWS Amplify, and how to add hosting and deploy a React application to a S3 bucket. We've seen that, by answering a few questions, we can use AWS Amplify to help provision all the necessary resources to host our application.

Note, deploying applications to any cloud provider can incur charges. Please remember to remove / delete any services that you do not wish to be charged for. Full details of AWS Amplify pricing can be found at https://aws.amazon.com/amplify/pricing/

If you've got any questions, please ask below in the comments.

## Credits

Photo by <a href="https://unsplash.com/@sergio_capuzzimati?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Sergio Capuzzimati</a> on <a href="/s/photos/amplify?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
