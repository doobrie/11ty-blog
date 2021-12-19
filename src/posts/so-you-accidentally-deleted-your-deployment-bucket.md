---
title: So you accidentally deleted your deployment bucket ... ?
date: 2021-03-19
tags: [aws, s3]
---

![](/images/cone.jpg)

## S3 Deployment Buckets

When developing applications with AWS Amplify, a S3 bucket is used to store all of the configuration files that are needed to define the back-end config.

This bucket is created when the application is initialized via the `amplify init` command.

![s3bucket.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1616189972547/xSkJ4Y3lj.png)

But what happens if you delete that bucket, either by accident, or because you archived the project and then came back to it at a later date? Well, when you try to deploy the back-end, the Amplify cli gives an error:

```bash
Could not initialize 'dev': The specified bucket does not exist
```

This happened to me recently, so I thought the simplest solution would be to recreate the bucket.

Within a project, Amplify stores details of the environment within the `amplify\team-provider-info.json` file. The deployment bucket is defined within the `DeploymentBucketName` entry within this file.

```json
{
  "dev": {
    "awscloudformation": {
      ...
      "DeploymentBucketName": "amplify-nextbestthing-dev-213017-deployment",
      ...
    }
  }
}
```

So, I recreated the bucket, give it the appropriate permissions, and tried again. I got further with the deployment, but given that the newly created bucket was empty, it's probably no surprise that I still couldn't deploy my application.

So, what next ?

Given that the environment is defined within the `amplify\team-provider-info.json` file in the project, I figured I had two options

1. Create a new environment profile
2. Delete the old environment profile and re-create it

## Recreating the Deployment Bucket

I like to keep code and environments clean, and didn't want to have a non working environment defined, so I decided to go with option (2) and delete the old environment (in my case called `dev`).

This was simply a matter of deleting the contents of the `amplify\team-provider-info.json` file and then executing `amplify init` to create a new environment..

My blank `amplify\team-provider-info.json` file looked like:

```json
{}
```

After running `amplify init`, I saw that a valid environment was created along with a new deployment bucket on S3.

Everything was back to normal and I could continue developing.

## Conclusion

S3 makes it difficult to accidentally delete a bucket, as you have to first ensure its empty and then type in the bucket name before being allowed to delete it. There are other, probably more valid, situations though where deleting a deployment bucket could have been done on purpose. Whatever the reason though, following the simple steps above will allow you to get going again in a short amount of time.

## Credits

Photo by <a href="https://unsplash.com/@lucian_alexe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Lucian Alexe</a> on <a href="/s/photos/accident?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
