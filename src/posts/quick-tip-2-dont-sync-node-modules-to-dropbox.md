---
title: Quick Tip No 2 - Don't sync node_modules to Dropbox
date: 2021-04-04
tags: [tip, node, dropbox]
---

![](/images/quick.jpg)

If you do any JavaScript development and store your files within a [Dropbox](https://www.dropbox.com/) folder, chances are that your PC is going to try to upload the contents of your `node_modules` folder into Dropbox. This is going to take a long time to upload and your actual application changes may not get uploaded when you want.

Fortunately, Dropbox has an option to specify which folders to sync, so its easy to configure the `node_modules` folder to remain local.

First of all, ensure that you are using the Dropbox application to view folders rather than in File Explorer. Open the Dropbox preferences, and ensure `Open folders in` is set to `Dropbox desktop app`.

![Dropbox preferences window](https://cdn.hashnode.com/res/hashnode/image/upload/v1617571412612/r6MXGENpM.png)

Next, locate the top level folder for your project (the one that contains `node_modules`), right click and select `Open In Dropbox`.

Within the Dropbox application, right click on the `node_modules` folder and select `Don't sync to dropbox.com`

![List of files in Dropbox application](https://cdn.hashnode.com/res/hashnode/image/upload/v1617571644437/bJs9eL7nV.png)

You'll now find syncing to Dropbox much faster.

## Credits

Photo by <a href="https://unsplash.com/@the_gerbs1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jean Gerber</a> on <a href="https://unsplash.com/@the_gerbs1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
